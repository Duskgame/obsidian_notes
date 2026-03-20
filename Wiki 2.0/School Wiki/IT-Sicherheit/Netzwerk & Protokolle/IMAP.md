---
aliases:
  - Internet Message Access Protocol
  - IMAP4
---
[RFC 9051 – Internet Message Access Protocol (IMAP) – Version 4rev2](https://www.rfc-editor.org/rfc/rfc9051)

**IMAP (Internet Message Access Protocol)** ist ein E-Mail-Protokoll, das den Zugriff auf E-Mails direkt auf dem Mailserver ermöglicht, ohne diese zwingend lokal herunterzuladen.

**Unterschied zu POP3:**

| IMAP | POP3 |
|---|---|
| E-Mails bleiben auf dem Server | E-Mails werden lokal heruntergeladen |
| Synchronisation über mehrere Geräte | Nur ein Gerät greift zu |
| Serverseitige Ordnerverwaltung | Lokale Ordner |
| Besser für Backup/Archivierung | Schlechter für zentrale Sicherung |

**Sicherheitsrelevanz:**
- Bei IMAP-Konfiguration werden E-Mails serverseitig gespeichert und durch die zentrale Datensicherung des Servers abgedeckt
- Lokale Speicherung (wie bei POP3 oder Thunderbird-Standard) erfordert gesonderte Client-Backups
- Verbindung zum Mailserver sollte über [[STARTTLS]] oder SSL/[[Transport Layer Security (TLS)|TLS]] verschlüsselt werden

**Im IT-Grundschutz:** APP.5.3.A3 fordert geregelte Datensicherung von E-Mails – IMAP vereinfacht dies erheblich.
