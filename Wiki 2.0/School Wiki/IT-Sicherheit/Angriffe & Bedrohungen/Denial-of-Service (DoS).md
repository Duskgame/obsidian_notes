---
aliases:
  - DoS
  - DDoS
  - Distributed Denial-of-Service
  - DoS-Angriff
---
> **Quelle:** [BSI – Bundesamt für Sicherheit in der Informationstechnik](https://www.bsi.bund.de/)

Ein **Denial-of-Service (DoS)-Angriff** ist ein Cyberangriff, bei dem ein Angreifer ein System, einen Dienst oder ein Netzwerk durch Überlastung gezielt außer Betrieb setzt, sodass legitime Nutzer nicht mehr darauf zugreifen können.

**Varianten:**
- **DoS (Denial-of-Service):** Angriff von einem einzigen System
- **DDoS (Distributed Denial-of-Service):** Koordinierter Angriff von vielen verteilten Systemen gleichzeitig (oft über Botnetze) — schwerer abzuwehren

**Angriffsmethoden:**
- **Volumetrisch:** Überschwemmung mit massivem Datenverkehr (z. B. UDP Flood, ICMP Flood)
- **Protokollbasiert:** Ausnutzung von Schwächen in Netzwerkprotokollen (z. B. SYN Flood)
- **Anwendungsschicht:** Gezielte Überlastung einer Anwendung (z. B. HTTP Flood gegen einen Webserver)

**Schutzmaßnahmen:**
- Rate Limiting und Traffic Filtering
- Firewalls und Intrusion Prevention Systems (IPS)
- Content Delivery Networks (CDN) zur Lastverteilung
- DDoS-Schutzdienste (z. B. Cloudflare, AWS Shield)

**Im IT-Grundschutz:** APP.5.3.A2 fordert, dass IT-Betrieb Schutzmechanismen gegen Denial-of-Service-Attacken auf E-Mail-Servern ergreift.
