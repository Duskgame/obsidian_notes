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


## Prozessablauf

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

### Cloud Functions (Gen 2, Python 3.11)
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
