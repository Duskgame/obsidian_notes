---
aliases:
  - Opportunistic TLS
  - STARTTLS-Erweiterung
---
> **Quelle:** [RFC 3207 – SMTP Service Extension for Secure SMTP over TLS](https://www.rfc-editor.org/rfc/rfc3207)

**STARTTLS** ist eine Protokollerweiterung, die es ermöglicht, eine zunächst unverschlüsselte Verbindung nachträglich auf eine verschlüsselte [[Transport Layer Security (TLS)|TLS]]-Verbindung hochzustufen (*Upgrade*). Für SMTP ist die Erweiterung in RFC 3207 spezifiziert.

**Funktionsweise:**
1. Client verbindet sich unverschlüsselt auf dem Standard-Port (z. B. SMTP Port 25 oder IMAP Port 143)
2. Client sendet den Befehl `STARTTLS`
3. Beide Seiten führen den TLS-Handshake durch
4. Ab diesem Punkt ist die Verbindung verschlüsselt

**Abgrenzung zu implizitem TLS:**
- **STARTTLS** (opportunistisch): Beginnt unverschlüsselt, wechselt dann zu TLS (Port 25/143/587)
- **Implizites TLS / SSL**: Verbindung ist von Anfang an verschlüsselt (Port 465/993) – sicherer, da kein unverschlüsselter Beginn

**Sicherheitshinweis:** STARTTLS kann durch einen [[Man-in-the-Middle]]-Angriff zum Downgrade auf unverschlüsselte Verbindungen missbraucht werden, wenn der Server keine Pflicht-TLS-Konfiguration erzwingt.

**Im IT-Grundschutz:** APP.5.3.A1 fordert, dass E-Mail-Clients sichere Transportverschlüsselung für die Kommunikation über nicht vertrauenswürdige Netze einsetzen.
