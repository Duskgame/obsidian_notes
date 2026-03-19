---
aliases:
  - MitM
  - Man-in-the-Middle-Angriff
  - MITM
---
Ein **Man-in-the-Middle-Angriff (MitM)** ist ein Angriff, bei dem sich ein Angreifer unbemerkt zwischen zwei kommunizierende Parteien schaltet und deren Datenverkehr mitlesen, manipulieren oder umleiten kann.

**Typische Szenarien:**
- Angreifer im selben WLAN fängt unverschlüsselten Datenverkehr ab (z. B. ARP-Spoofing)
- Gefälschtes Root-Zertifikat ermöglicht das Entschlüsseln von HTTPS-Verbindungen
- DNS-Spoofing leitet Benutzer auf gefälschte Webseiten um

**Schutzmaßnahmen:**
- Verschlüsselung aller Verbindungen ([[Transport Layer Security (TLS)|TLS]])
- [[HTTP Strict Transport Security (HSTS)|HSTS]] verhindert Downgrade-Angriffe
- Kontrolle der vertrauenswürdigen [[Root Certificate|Root-Zertifikate]]
- [[DNS over HTTPS|DNS-over-HTTPS]] verhindert DNS-Manipulation

**Im IT-Grundschutz:** Relevant für APP.1.2.A3 (vertrauenswürdige Zertifikate) und APP.5.3.A1/A2 (E-Mail-Verschlüsselung).
