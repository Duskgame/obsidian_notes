## Was ist SAKE?

SAKE (GCP Service Account Key Rotation Helper) ist ein internes Tool von bonprix zur Verwaltung und sicheren Auslieferung von GCP Service Account Keys. Es besteht aus einer statischen Webanwendung (SvelteKit) und mehreren Cloud Functions (Python).

Erreichbar unter: `sake.dev-key-rotation.gcns.bonprix.net`
Zugang: IAP-geschützt, nur `@bonprix.net`-Accounts


## Die zwei Systeme

Es existieren aktuell zwei parallele Systeme:

| | Crawler (alt) | SAKE (neu) |
|---|---|---|
| Typ | Python-Skript | Web-App + Cloud Functions |
| Scope | `bpde-*`, nur `CoreDWH BigQuery`-Folder | Geplant: gesamte Organisation |
| Key-Auslieferung | ZIP-Datei, Passwort via `secret.sec.otto.de` | Browser-Crypto, verschlüsselt mit Public Key des Requesters |
| Trigger | Job-Files in GCS-Bucket | Manuell via Web-UI |
| Status | Produktiv | In Entwicklung (Repo: `wip-gcp-project-key-rotation`) |

SAKE soll den Crawler langfristig ablösen oder auf die gesamte Organisation ausweiten.


## Redesigned flow — SAKE v2 (wip-gcp-project-key-rotation_test2)

The original flow required the supporter to hand back `key_id` and `client_id` to the requester after activating the cert. The redesigned flow eliminates this round-trip entirely.

**Key insight:** The GCP Key ID is the SHA-1 hash of the full PEM certificate + a trailing newline. This can be computed locally in the browser before the cert is ever uploaded to GCP:

```typescript
// Key ID computed locally — no GCP API call needed
computedKeyId = await sha1(certPem + '\n');
```

The requester also provides the **OAuth2 Client ID** upfront in the form (Step 1). With both values known locally, the SAK JSON can be assembled immediately after cert generation — before the supporter does anything.

### New requester flow (2 steps)

**Step 1 — Fill form:**
- Jira Ticket, Service Account email, OAuth2 Client ID, expiry days
- OAuth2 Client ID: found in GCP console under the SA details, or provided by team lead

**Step 2 — Done:**
- Browser generates RSA keypair + self-signed X.509 cert (`pkijs`)
- Key ID computed via `sha1(certPem + "\n")`
- SAK JSON assembled immediately and ready to download
- Activation link generated (cert embedded in URL hash `#cert=...`)
- Requester downloads SAK and sends the activation link to a supporter
- **No waiting. No hand-back.**

### New supporter flow (2 steps)

**Step 1 — Review:**
- Opens activation link (cert + metadata pre-filled from URL)
- Chooses activation method: browser (Google login) or CLI

**Step 2a — Browser activation:**
- Authenticates via Google OAuth2 (GAPI client)
- Views and optionally deletes existing keys on the SA
- Clicks "Upload & Activate" → `iam.projects.serviceAccounts.keys.upload()`

**Step 2b — CLI activation:**
```bash
TMPF=$(mktemp) && \
echo -e "-----BEGIN CERTIFICATE-----\n<cert>" > $TMPF && \
gcloud --project <project> \
  iam service-accounts keys upload $TMPF \
  --iam-account=<sa-email> && \
rm $TMPF
```
Clicks "Command ran successfully" to proceed to the Done screen.

**Step 3 — Done:**
- "The requester already has their Service Account Key — no need to send anything back."
- Supporter records SA mail, Jira ticket, activation date for their own reference.

### Flow comparison

| | Original flow | New flow (v2) |
|---|---|---|
| OAuth2 Client ID | Returned by supporter after upload | Entered by requester upfront |
| Key ID | Returned by GCP after upload | Computed locally: `sha1(cert + "\n")` |
| SAK assembly | Step 3 (after supporter hand-back) | Step 2 (immediately after cert gen) |
| Supporter hand-back | Required (key_id + client_id) | Not needed |
| Round-trips | 2 (cert → supporter → back) | 1 (cert → supporter, done) |

---

## Prozessablauf (Original — wip-gcp-project-key-rotation)

### Rolle: Requester
1. Öffnet SAKE-Webapp
2. Füllt Formular aus: Jira-Ticket, SA-Mail, Ablauf in Tagen (Standard: 90)
3. Browser generiert X.509-Keypair via `pkijs` / Web Crypto API
   - Privater Key bleibt im Browser-Speicher
   - Zertifikat (Public Key) wird für den Supporter vorbereitet
4. Lädt Zertifikat herunter, schickt Link mit Zertifikat an Supporter
5. Wartet auf Key ID + OAuth2 Client ID vom Supporter zurück
6. Gibt diese in SAKE ein → Browser entschlüsselt den SAK lokal

> Der private Key existiert nur im Browser. Bei Schließen des Tabs muss der Prozess neu gestartet werden.

### Rolle: Supporter (privilegierter GCP-Zugang)
**Szenario A — Browser:**
1. Öffnet Link (Zertifikat wird via `#cert`-URL-Parameter vorausgefüllt)
2. Meldet sich via Google Login an (Browser macht GCP API-Calls direkt)
3. Löscht abgelaufene Keys
4. Aktiviert das Zertifikat des Requesters via Google API
5. Kopiert Informationen manuell ins Jira Ticket

**Szenario B — Google CLI:** *(unvollständig spezifiziert)*
1. Öffnet Google Console manuell
2. Kopiert CLI-Befehl, führt ihn lokal aus
3. CLI-Output manuell ins Jira Ticket kopieren


## Technische Komponenten

### Frontend
- SvelteKit 2 + Svelte 5, TypeScript, Tailwind CSS
- Rein statisch (`adapter-static`, `prerender = true`, `ssr = false`)
- Hosting: Cloud Run (Nginx in Docker) — nur als statischer Fileserver
- Crypto: `pkijs`, `asn1js`, `pvutils` für X.509 im Browser
- CI/CD: Cloud Build via GitHub Actions OIDC

### [[Cloud Functions Gen2]] (Python 3.11)
| Function | Aufgabe | Status |
|---|---|---|
| `smtpsender` | E-Mail mit Anhang via SMTP | Deployed |
| `jiracommenter` | Kommentare auf Jira Tickets | Deployed |
| `adgroups` | AD-Gruppen-Lookup via signJwt | Deployed |
| `key-rotator` | Key-Operationen via IAM API | **Fehlt komplett** |

### Storage
- `jobs_bucket` — Job-Files für Crawler (create/delete/renew)
- `files_bucket` — Verschlüsselte ZIP-Dateien mit Keys (Crawler)
- `terraform` — Terraform State
- `cloudfunction_sources` — CF Sourcecode

### Infrastruktur
- Interner Load Balancer (INTERNAL_MANAGED, europe-west1)
- TLS 1.2 (RESTRICTED Profile), Let's Encrypt
- GCP Projekte: `bpde-dev-key-rotation` (Dev), `bpde-prd-key-rotation` (Prod)


## Was ist fertig / was fehlt

### Fertig
- Frontend (SvelteKit, Crypto im Browser)
- IAP-Schutz und Cloud Run Hosting
- E-Mail-Versand (smtpsender CF)
- Jira-Kommentare (jiracommenter CF)
- AD-Gruppen-Lookup (adgroups CF)
- Infrastruktur (Load Balancer, TLS, DNS)

### Fehlt / unvollständig
- `key-rotator` Cloud Function (Routing existiert, kein Code)
- `sake-webapp` Service Account hat keine IAM-Rechte für Key-Operationen
- Key ID Verifikation (Schritt 25 im Flow) nicht implementiert
- Cloud Scheduler für automatischen Rotations-Trigger
- Grace Period beim Löschen alter Keys
- Szenario B (CLI-Pfad) unvollständig
- `BASE_PATH` Environment Variable nicht in Terraform gesetzt


## Bekannte Probleme

### Sicherheit (vor nächstem Deployment beheben)
- `smtpsender` gibt SMTP-Credentials im HTTP-Response zurück (Debug-Code)
- `jiracommenter` loggt kompletten Request-Body inkl. Passwörter
- ZIP-Passwort in Crawler `main.py` ist der Literal-String `"passwort"` (in `manual.py` zufällig generiert)

### Personenabhängigkeiten
- `stephan.jauernick@bonprix.net` als `ADMIN_USER` in Terraform hardcoded
- `moritz.colaci@bonprix.net` in Crawler-Code für Bucket-Zugriff hardcoded
- Keine Vertretungsregelung dokumentiert

### Technisch
- `node: ">=10"` in `package.json` (EOL)
- `krcf` Dummy-Eintrag (`dummy123`) in `shared-lb.tf` vergessen
- Externe Abhängigkeit: `secret.sec.otto.de` (Otto-Infrastruktur, 7-Tage-Ablauf) — keine SLA bekannt


---

## In-Depth: Technische Tiefen

### Kryptografie im Browser — wie der Key-Exchange wirklich funktioniert

Der Kernmechanismus von SAKE ist ein **Zero-Knowledge Key Delivery**: GCP bekommt nur den Public Key des Requesters, nie den privaten Schlüssel. Der Requester entschlüsselt lokal im Browser.

**Schritt 1 — Requester generiert X.509 Keypair** (`/request`-Route, `handleCert()`):
```
Browser → pkijs / Web Crypto API
  → RSA-2048 Keypair generieren (privateKey bleibt in JS-Memory)
  → Self-signed X.509 Zertifikat erstellen
      Subject CN  = Service-Account-Email
      Subject O   = Jira-Ticket-Nummer
      Validity    = today + expiry_days (Standard: 90 Tage)
  → PEM-kodiertes Zertifikat (Base64-wrapped) zum Download / Clipboard
```

Das Zertifikat kodiert keine sensiblen Daten — es enthält nur den Public Key + Metadaten. Der Private Key existiert ausschließlich im Browser-Tab und wird nie übertragen.

**Schritt 2 — Supporter lädt Zertifikat hoch** (`/upload`-Route, `handleGCPUpload()`):
```
Browser → Google OAuth2 (GAPI-Client, Client-ID: 162427647549-...)
  → IAM API: projects.serviceAccounts.keys.upload()
      body: { publicKeyData: <base64-encoded PEM> }
  → GCP antwortet mit:
      keyId    = unique ID des neu angelegten SA-Keys
      clientId = OAuth2 Client-ID des SA (für SAK JSON-Format nötig)
```

GCP speichert nur den Public Key. Es gibt keinen korrespondierenden Private Key auf GCP-Seite — der kommt vom Requester-Browser.

**Schritt 3 — Requester rekonstruiert den SAK** (`generateSKF()`):
```
Browser → kombiniert eigenen privateKey (aus Step 1, noch im Tab-Speicher)
         mit keyId + clientId vom Supporter
  → erzeugt Standard-GCP Service Account Key JSON:
      {
        "type": "service_account",
        "project_id": "<parsed from SA email>",
        "private_key_id": "<keyId>",
        "private_key": "<PEM private key>",
        "client_email": "<SA email>",
        "client_id": "<clientId>",
        "auth_uri": "https://accounts.google.com/o/oauth2/auth",
        "token_uri": "https://oauth2.googleapis.com/token"
      }
```

Das JSON ist identisch mit einem per `gcloud iam service-accounts keys create` erzeugten Key — jedes Tool (ADC, gcloud, SDKs) akzeptiert es ohne Änderung. Der Witz: GCP hat den Private Key nie gesehen.

**Warum dieses Design?**
GCP selbst bietet keine Möglichkeit, einen eigenen Private Key als SA-Key hochzuladen (die API `keys.upload` nimmt nur Public Keys). SAKE nutzt genau das aus: Der Requester bringt sein eigenes Keypair mit, GCP registriert nur den Public-Key-Teil. Kein Zwischenserver speichert den privaten Key.


### Crawler — wie der Legacy-Workflow technisch abläuft

**Service Account Discovery** (`list_projects()` + `list_service_accounts()`):
```python
# Cloud Resource Manager API — listet alle GCP-Projekte der Org
resource_manager = build("cloudresourcemanager", "v1")
projects = resource_manager.projects().list().execute()

# IAM API — listet alle SAs pro Projekt
iam = build("iam", "v1")
service_accounts = iam.projects().serviceAccounts().list(name=f"projects/{project_id}").execute()
```

**Kontakt-E-Mail-Parsing** (`get_contact_email()`):
SA-Descriptions müssen einem bestimmten Format folgen:
```
purpose:Batch ETL Job;Contact:team-dwh@bonprix.net
```
Der Crawler parst mit Regex `(?<=Contact:)[^\s;]+` die E-Mail aus der Description. Ohne korrekte Description → kein automatischer Kontakt → kein Key-Versand.

**Key-Alter-Prüfung**:
```python
ROTATION_PERIOD = 180  # Tage
PREWARNING_DAYS = 5    # Vorab-Warnung

key_age = (now - key_created_at).days
if key_age >= (ROTATION_PERIOD - PREWARNING_DAYS):  # 175 Tage
    # Erneuerungs-Job-File in GCS anlegen
```

**Key-Verteilung via ZIP + Passwort** (`create_key_zipfile()` + `create_secret_link_otto()`):
```python
# Passwort zufällig generieren (manual.py — Crawler main.py hat Bug: "passwort" literal!)
password = create_password()  # 20 Zeichen, Buchstaben+Ziffern+Sonderzeichen

# ZIP mit Passwort schützen
with ZipFile(buffer, "w") as zf:
    zf.setpassword(password.encode())
    zf.write(key_json, arcname="key.json")

# Passwort sicher teilen via Otto Secret Service
response = requests.post("https://secret.sec.otto.de/secret/create", json={
    "secret": password,
    "lifetime": 10080  # 7 Tage in Minuten
})
secret_link = response.json()["link"]
# Link enthält ein Einmal-Token — Empfänger öffnet Link, sieht Passwort einmalig
```

Die ZIP-Datei landet in `prd_key_rotation_key_files` in GCS mit expliziten ACL-Permissions für `moritz.colaci@bonprix.net` (hardcoded Bug).


### AD-Gruppen-Lookup — [[Domain-Wide Delegation]] ohne Static Keys

Die `adgroups`-Function ist ein gutes Beispiel für GCP-native Auth ohne Service-Account-Keys:

```python
def finaggle_domain_sharing_credential(user_email: str) -> str:
    # 1. JWT selbst bauen (ohne google-auth-library)
    jwt_payload = {
        "iss": service_account_email,       # SA-E-Mail
        "sub": ADMIN_USER,                  # impersonierter Admin
        "scope": "https://www.googleapis.com/auth/admin.directory.group.readonly",
        "aud": "https://oauth2.googleapis.com/token",
        "exp": now + 3600,
        "iat": now
    }
    
    # 2. JWT via IAM Credentials API signieren (kein Private Key lokal nötig!)
    iam_creds = build("iamcredentials", "v1")
    signed = iam_creds.projects().serviceAccounts().signJwt(
        name=f"projects/-/serviceAccounts/{service_account_email}",
        body={"payload": json.dumps(jwt_payload)}
    ).execute()
    
    # 3. Signierten JWT gegen OAuth2-Token tauschen
    token_response = requests.post("https://oauth2.googleapis.com/token", data={
        "grant_type": "urn:ietf:params:oauth2:grant-type:jwt-bearer",
        "assertion": signed["signedJwt"]
    })
    return token_response.json()["access_token"]
```

**Warum `signJwt` statt Private Key?**: Die Function braucht keinen SA-Key im Filesystem. Stattdessen signiert GCP das JWT intern via IAM Credentials API. Die Function benötigt dafür die IAM-Rolle `roles/iam.serviceAccountTokenCreator` auf sich selbst.


### Infrastruktur — Load Balancer & TLS im Detail

**Netzwerk-Topologie**:
```
Internet → [HTTP :80] → HTTP-Redirect-Proxy → HTTPS
         → [HTTPS :443] → SSL-Proxy (TLS 1.2, RESTRICTED-Policy)
                        → Internal L7 Load Balancer
                        → URL Map (path-based routing):
                            /smtpsender/* → Cloud Function smtpsender
                            /jiracommenter/* → Cloud Function jiracommenter
                            /adgroups/* → Cloud Function adgroups
                            /* (default) → Cloud Run sake-webapp
```

**TLS via Let's Encrypt (ACME-Protokoll in Terraform)**:
```hcl
# acme_provider registriert sich bei Let's Encrypt
resource "acme_certificate" "sake_cert" {
  dns_challenge {
    provider = "gcloud"  # DNS-01 Challenge via Cloud DNS
  }
  common_name = "*.dev-key-rotation.gcns.bonprix.net"
  min_days_remaining = 30  # Terraform erneuert automatisch bei < 30 Tagen
}
```

Der Terraform-State speichert den privaten TLS-Schlüssel des Zertifikats — das ist ein häufiger Kritikpunkt an dieser Terraform-ACME-Lösung.

**[[Identity-Aware Proxy (IAP)]]**:
```hcl
resource "google_iap_web_backend_service_iam_member" "sake_iap" {
  role   = "roles/iap.httpsResourceAccessor"
  member = "domain:bonprix.net"  # Alle @bonprix.net-Accounts
}
```
IAP sitzt vor dem Load Balancer und prüft Google-Identity. Nur authentifizierte `@bonprix.net`-Accounts kommen durch. Kein Login → 401/Redirect zu Google Login.

**VPC Connector** (`con-dev-euw1-srvless01`):
Cloud Functions und Cloud Run sind Serverless-Dienste, haben keinen direkten VPC-Zugang. Der Connector tunnelt deren ausgehenden Traffic ins interne VPC-Netzwerk. `ALL_TRAFFIC`-Egress bedeutet: auch Internet-Traffic geht über den Connector und damit über die interne Infrastruktur.


### CI/CD — OIDC statt Static Keys für GitHub Actions

**Das Problem mit traditionellen Ansätzen**: GitHub Actions müsste einen SA-Key als Secret speichern — genau das, was SAKE vermeiden will.

**Die OIDC-Lösung**:
```yaml
# .github/workflows/dev.yaml
- uses: google-github-actions/auth@v2
  with:
    workload_identity_provider: >
      projects/328161350925/locations/global/workloadIdentityPools/
      github-oidc-pool/providers/github-oidc-provider
    service_account: infrastructureascode@bpde-dev-key-rotation.iam.gserviceaccount.com
```

**Ablauf**:
1. GitHub Actions erzeugt einen signierten OIDC-Token (JWT) für den Workflow
2. Token enthält Claims wie `repository`, `ref`, `actor`
3. [[Workload Identity Federation]] validiert den Token via GitHub's JWKS-Endpoint
4. Der Workload Identity Pool prüft Attribute: nur `refs/heads/dev` (bzw. `main`) darf sich als `infrastructureascode` authentifizieren
5. GCP tauscht den OIDC-Token gegen ein kurzlebiges Access-Token für den SA

Kein statischer Key wird je erstellt oder in GitHub gespeichert. Das Access-Token gilt nur für die Dauer des Workflow-Jobs.

**Zwei SA-Rollen**:
- `infrastructureascode` — Owner-Permissions, nur für `main`-Branch (Terraform Apply)
- `infrastructureascode-viewer` — Read-Only, für PRs (Terraform Plan/Validate)


### Cloud Functions Deployment-Pipeline

**Wie Code zu einer laufenden Cloud Function wird**:

```
webapp/cloudbuild.yaml → Cloud Build Steps:
  1. docker build → Image: europe-west1-docker.pkg.dev/{PROJECT_ID}/sake-images/{SERVICE}/{BUILD_ID}
  2. docker push → Artifact Registry
  3. gcloud run deploy → Cloud Run Service (europe-west1)

infrastructure/dev/cloudfunctions.tf:
  1. archive_file → ZIP der Python-Sourcen
  2. google_storage_bucket_object → ZIP in cloudfunction_sources-Bucket
  3. google_cloudfunctions2_function → Cloud Function Gen2
       runtime: python311
       entry_point: hello_http
       source: {bucket}/{zip-object}
       service_config:
         memory: 256MB
         timeout: 60s
         max_instances: 10
         vpc_connector: con-dev-euw1-srvless01
         ingress: ALLOW_INTERNAL_AND_GCLB  # Kein direkter Internet-Zugang
```

**Gen2 Cloud Functions** sind intern Cloud Run-Dienste. Der einzige Unterschied zum manuellen Cloud Run-Deployment: GCP verwaltet das Container-Image automatisch aus dem Python-Sourcecode.


### Fehlende `key-rotator` Cloud Function — was sie tun müsste

Laut Routing in `shared-lb.tf` gibt es einen `/krcf`-Pfad (Key Rotation Cloud Function), aber keinen Deployment-Code. Basierend auf dem restlichen Code wäre die Implementierung:

```python
# Erwartetes Interface (POST /krcf)
# Input: { "samail": "...", "cert_pem": "...", "jira_ticket": "..." }
# Ablauf:
# 1. Zertifikat validieren (Signatur, Ablaufdatum, CN = SA-Mail)
# 2. IAM API: projects.serviceAccounts.keys.upload(publicKeyData=cert_pem)
# 3. Alte Keys auflisten + ggf. löschen (Grace Period!)
# 4. Jira-Kommentar via jiracommenter-CF
# 5. Response: { "key_id": "...", "client_id": "..." }
```

Der `sake-webapp`-SA hat aktuell keine `iam.serviceAccountKeys.*`-Permissions — auch diese müssten per Terraform vergeben werden.


### Service Account Email Format — warum das Regex wichtig ist

GCP SA-Emails haben das Format:
```
{name}@{project-id}.iam.gserviceaccount.com
```

`decomposeServiceAccount()` im Frontend (und analog im Crawler):
```typescript
const regex = /^(?<name>.*)@(?<project>.*).iam.gserviceaccount.com$/;
const match = saMail.match(regex);
// match.groups.name    → SA-Name
// match.groups.project → GCP-Projekt-ID
// path                 → "projects/{project}/serviceAccounts/{saMail}"
```

Der `path` wird direkt in IAM API-Aufrufen verwendet:
```
GET https://iam.googleapis.com/v1/projects/{project}/serviceAccounts/{saMail}/keys
```

Der Crawler validiert zusätzlich gegen ein strengeres Regex:
```python
r"([a-z0-9-]{6,30})@bpde-([a-z0-9-]+)\.iam\.gserviceaccount\.com"
```
→ nur bonprix-eigene SAs (`bpde-*`-Projekte), Namen 6–30 Zeichen, nur Kleinbuchstaben/Ziffern/Bindestriche.
