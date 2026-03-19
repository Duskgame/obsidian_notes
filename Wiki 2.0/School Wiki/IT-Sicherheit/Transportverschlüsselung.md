---
aliases:
  - Transport Layer Encryption
  - Verschlüsselung in Transit
  - sichere Transportverschlüsselung
---
**Transportverschlüsselung** bezeichnet die Verschlüsselung von Daten während ihrer Übertragung über ein Netzwerk, um sie vor unbefugtem Mitlesen oder Manipulation zu schützen.

**Abgrenzung:**
- **Transportverschlüsselung (in Transit):** Daten werden verschlüsselt übertragen, liegen auf dem Server jedoch unverschlüsselt vor — der Serverbetreiber kann theoretisch auf die Daten zugreifen
- **Ende-zu-Ende-Verschlüsselung (E2EE):** Daten werden nur beim Sender ver- und beim Empfänger entschlüsselt — auch der Serverbetreiber hat keinen Zugriff

**Gängige Protokolle:**
- **[[Transport Layer Security (TLS)|TLS]]** (Transport Layer Security): Standard für HTTPS, SMTPS, IMAPS, etc.
- **[[STARTTLS]]:** Nachträglicher Upgrade einer unverschlüsselten Verbindung auf TLS

**Bei E-Mail-Kommunikation:**
- **Client → Server:** E-Mail-Client verbindet sich verschlüsselt zum Mailserver (IMAPS Port 993, SMTPS Port 465/587)
- **Server → Server:** Mailserver kommunizieren untereinander verschlüsselt via STARTTLS oder implizitem TLS

**Im IT-Grundschutz:** APP.5.3.A1 und APP.5.3.A2 fordern, dass E-Mail-Clients und -Server für die Kommunikation über nicht vertrauenswürdige Netze sichere Transportverschlüsselung einsetzen.
