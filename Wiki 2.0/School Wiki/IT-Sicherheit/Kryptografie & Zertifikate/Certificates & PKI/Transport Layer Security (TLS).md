---
aliases:
  - TLS
  - SSL
  - SSL/TLS
  - HTTPS
  - TLS-Handschlag
  - TLS-Handshake
---
[RFC 8446 – The Transport Layer Security (TLS) Protocol Version 1.3](https://www.rfc-editor.org/rfc/rfc8446)

**Transport Layer Security (TLS)** ist das wichtigste kryptografische Protokoll zur sicheren Übertragung von Daten über Netzwerke. Es schützt Verbindungen durch Verschlüsselung, Authentifizierung und Integritätssicherung.

**TLS-Versionen:**

| Version | Status |
|---------|--------|
| SSL 2.0 / 3.0 | Veraltet, unsicher (POODLE, DROWN) |
| TLS 1.0 | Veraltet, deaktiviert (z. B. Chrome 84+) |
| TLS 1.1 | Veraltet, deaktiviert |
| TLS 1.2 | Weit verbreitet, sicher bei korrekter Konfiguration |
| TLS 1.3 | Aktueller Standard — schneller, sicherer |

**TLS-Handshake (vereinfacht):**
1. Client sendet unterstützte TLS-Versionen und Cipher Suites
2. Server wählt Version und Cipher, sendet Zertifikat
3. Client prüft Zertifikat (Signatur, Gültigkeit, Widerruf via [[OCSP (Online Certificate Status Protocol)|OCSP]]/[[CRL (Certificate Revocation List)|CRL]])
4. Schlüsselaustausch → gemeinsamer Sitzungsschlüssel
5. Verschlüsselte Verbindung aufgebaut

**Einsatz:**
- **HTTPS** = HTTP über TLS (Port 443)
- **SMTPS/STARTTLS** = E-Mail über TLS → [[Transportverschlüsselung]]
- **IMAPS** = IMAP über TLS (Port 993) → [[IMAP]]
- **LDAPS** = LDAP über TLS

**Im IT-Grundschutz:** APP.1.2.A2 fordert TLS in sicherer Version; APP.5.3.A1/A2 fordern TLS für E-Mail-Verbindungen.
