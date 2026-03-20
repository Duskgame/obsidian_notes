---
aliases:
  - HSTS
  - Strict Transport Security
---
[RFC 6797 – HTTP Strict Transport Security (HSTS)](https://www.rfc-editor.org/rfc/rfc6797)

**HTTP Strict Transport Security (HSTS)** ist ein Sicherheitsmechanismus, der Browser anweist, eine bestimmte Website ausschließlich über verschlüsselte [[Transport Layer Security (TLS)|HTTPS]]-Verbindungen zu laden und HTTP-Verbindungen automatisch abzulehnen.

**Funktionsweise:**
- Server sendet den Header: `Strict-Transport-Security: max-age=31536000; includeSubDomains`
- Browser speichert diese Anweisung für die angegebene Dauer (z. B. 1 Jahr)
- Bei zukünftigen Aufrufen der Domain leitet der Browser automatisch auf HTTPS um — noch bevor eine Verbindung hergestellt wird
- **HSTS Preload:** Domains können in eine browserinterne Liste aufgenommen werden — HSTS gilt dann auch beim allerersten Besuch

**Schutz vor:**
- **[[Man-in-the-Middle|SSL-Stripping-Angriffe]]:** Ein Angreifer kann die HTTPS-Verbindung nicht auf HTTP downgraden
- **Unbeabsichtigte HTTP-Aufrufe:** Verhindert versehentliche unverschlüsselte Verbindungen

**Grenzen:**
- Schützt nicht beim ersten Besuch einer Seite (ohne Preload)
- Gilt nur für die gespeicherte Domain — Subdomains brauchen `includeSubDomains`

**Im IT-Grundschutz:** APP.1.2.A2 fordert, dass eingesetzte Webbrowser HSTS gemäß RFC 6797 unterstützen und einsetzen.
