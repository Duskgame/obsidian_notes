---
aliases:
  - DoH
  - DNS over HTTPS
  - DNS-over-HTTPS
---
> **Quelle:** [RFC 8484 – DNS Queries over HTTPS (DoH)](https://www.rfc-editor.org/rfc/rfc8484)

**DNS over HTTPS (DoH)** ist ein Protokoll, das DNS-Anfragen über verschlüsselte HTTPS-Verbindungen überträgt, anstatt sie unverschlüsselt über UDP/Port 53 zu senden.

**Problem mit klassischem DNS:**
- DNS-Anfragen werden unverschlüsselt übertragen
- Netzwerkbetreiber, ISPs und Angreifer können sehen, welche Domains aufgerufen werden
- DNS-Antworten können manipuliert werden ([[Man-in-the-Middle|DNS Spoofing]])

**Funktionsweise:**
- DNS-Anfragen werden als HTTPS-Requests an einen DoH-Resolver gesendet (z. B. `https://cloudflare-dns.com/dns-query`)
- Transport ist durch [[Transport Layer Security (TLS)|TLS]] verschlüsselt und authentifiziert
- Für Netzwerkbeobachter nicht von normalem HTTPS-Traffic unterscheidbar

**Einsatz in Browsern:**
- Firefox und Chrome unterstützen DoH nativ (Secure DNS / DNS over HTTPS-Einstellung)
- Kann auf bestimmten DoH-Resolver konfiguriert werden oder automatisch erkannt werden

**Kontroverse:**
- Netzwerkadministratoren verlieren Sichtbarkeit auf DNS-Ebene (Filterung, Logging)
- Zentralisierung bei großen DoH-Providern möglich
- Im Unternehmensumfeld oft per [[Group Policy Object (GPO)|GPO]] geregelt

**Im IT-Grundschutz:** APP.1.2.A2 fordert, dass Webbrowser aktuelle Sicherheitsstandards für Verbindungen unterstützen — DoH schützt dabei die DNS-Kommunikation.
