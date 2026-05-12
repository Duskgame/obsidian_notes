
## Warum Service Account Keys?

Google selbst empfiehlt SA Keys nur als letzten Ausweg. ([Best Practices](https://docs.cloud.google.com/iam/docs/best-practices-for-managing-service-account-keys), [Auth Decision Tree](https://docs.cloud.google.com/docs/authentication#auth-decision-tree))

- Ich habe gehört dass die Keys für „custom applications" gebraucht werden — wo laufen diese Anwendungen konkret? Auf GCP selbst, on-premise, oder woanders?
- Haben diese Umgebungen einen eigenen Identity Provider (z.B. AWS IAM, GitHub Actions, Azure AD)?
- Warum SA Keys und nicht Workload Identity Federation?
- Gibt es eine dokumentierte Entscheidung dazu?
- Gibt es ein Dokument das erklärt warum der Prozess so gebaut wurde?
- Wurde das mit dem Security-Team abgestimmt?


## Was ist der aktuelle Zustand?

- Wo ist dokumentiert welche Service Accounts aktive Keys haben?
- Wie alt sind die bestehenden Keys? ([Key Rotation](https://docs.cloud.google.com/iam/docs/key-rotation))
- Gibt es Keys die abgelaufen sind oder nie rotiert wurden?
- Wer ist aktuell für die Rotation zuständig — der Crawler, SAKE, oder beides?
- Soll SAKE den Crawler langfristig ablösen oder parallel laufen?
- Der Crawler deckt nur `CoreDWH BigQuery` ab — soll SAKE das auf die gesamte Organisation ausweiten?


## Wie funktioniert SAKE genau?

### Das Zertifikat (Cert)

*Kontext: Der Requester generiert in Schritt 1.1 im Browser ein X.509-Keypair via `pkijs` / Web Crypto API. Der private Key bleibt lokal im Browser-Speicher, das Zertifikat (Public Key) geht an den Supporter. Der Supporter registriert den Public Key bei GCP — damit verlässt der private Key niemals GCP.*

- Was ist das „Cert" genau — ist das der Key selbst oder etwas anderes?
- Laut Code lebt der private Key ausschließlich im Browser-Speicher — ich habe aber gehört das sei nicht so. Was stimmt?
- Wenn der Browser geschlossen wird — muss der gesamte Prozess neu gestartet werden?

### OAuth2 Client ID

*Kontext: In Schritt 2 gibt es ein Feld „SA OAuth2 ClientID" das in der Standard-GCP-Dokumentation für normale Service Account Keys nicht auftaucht.*

- Was ist die „SA OAuth2 ClientID" — Standard-GCP oder SAKE-spezifisch?

### Der Supporter-Schritt

*Kontext: Laut Code macht der Supporter alle GCP API-Calls direkt im Browser über seinen eigenen Google-Login. Der `sake-webapp` Service Account hat aktuell keine IAM-Rechte für Key-Operationen.*

- Soll die Webapp selbst die Key-Operationen übernehmen oder bleibt das in Cloud Functions?
- Was passiert bei „Ready: false" — Fehlermeldung oder bleibt es einfach so?
- Wer genau ist der Supporter — `stephan.jauernick` und `moritz.colaci` sind im Code hardcoded, ist das so geplant?

### Der manuelle Prozess

- Warum muss die Rotation manuell angestoßen werden — ist das eine technische oder regulatorische Entscheidung?
- Was passiert wenn der Supporter im Urlaub ist wenn ein Key abläuft?
- Wann soll die `key-rotator` Cloud Function implementiert werden — die fehlt noch komplett?

### Die zwei Szenarien (Browser vs. CLI)

- Wann wird Browser-Weg, wann CLI-Weg genutzt?
- Szenario B (CLI) sieht unfertig aus — ist das noch in Planung oder wird es gestrichen?


## Was passiert nach der Key-Auslieferung?

- Wie kommt der Key vom Rechner des Requesters in die Anwendung?
- Ist es gewollt dass der Key als Datei auf dem lokalen Rechner landet?
- Was ist der vorgesehene Speicherort — wenn nicht Dateisystem und nicht Secret Manager? ([Best Practices](https://docs.cloud.google.com/iam/docs/best-practices-for-managing-service-account-keys#protecting-against-privilege-escalation))
- Wie wird sichergestellt dass der alte Key nach der Rotation deaktiviert und gelöscht wird — gibt es eine Grace Period?
- Gibt es ein Monitoring das erkennt wenn ein abgelaufener Key noch genutzt wird?


## Sicherheit und Compliance

- Ist Key-Erstellung außerhalb von SAKE durch eine Organization Policy eingeschränkt? ([Best Practices](https://docs.cloud.google.com/iam/docs/best-practices-for-managing-service-account-keys#protecting-against-credential-leakage))
- Ist „Service Account Key Exposure Response" auf DISABLE_KEY gesetzt? ([Best Practices](https://docs.cloud.google.com/iam/docs/best-practices-for-managing-service-account-keys#protecting-against-credential-leakage))
- Wird pro Anwendung ein eigener Key ausgestellt oder kann ein Key für mehrere Systeme beantragt werden? ([Best Practices](https://docs.cloud.google.com/iam/docs/best-practices-for-managing-service-account-keys))
- Das ZIP-Passwort im Crawler `main.py` ist der String `"passwort"` — welche Version läuft in Produktion, `main.py` oder `manual.py`?
- Was ist `secret.sec.otto.de` genau und gibt es eine SLA falls das nicht erreichbar ist?


## Jira

- Kommt das Jira-Ticket am Anfang oder am Ende des Prozesses?
- Ist der manuelle Jira-Eintrag durch den Supporter der geplante Endzustand oder eine Übergangslösung?


## Roadmap

- Das Repo heißt noch `wip-gcp-project-key-rotation` — gibt es einen definierten MVP-Scope?
- Was sind die nächsten geplanten Schritte?


## User

- Um welche User geht es genau?
- Wofür benutzen die User die Keys?
- Wo legen die User die Keys ab und wie wird darauf zugegriffen?
- Gibt es einen Unterschied zwischen internen Usern und externen Systemen?
