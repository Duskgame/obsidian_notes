---
aliases:
  - Erpressungssoftware
  - Verschlüsselungstrojaner
---
[BSI – Ransomware](https://www.bsi.bund.de/DE/Themen/Verbraucherinnen-und-Verbraucher/Cyber-Sicherheitslage/Methoden-der-Cyber-Kriminellen/Ransomware/ransomware_node.html)

**Ransomware** ist eine Art von Schadsoftware (Malware), die Dateien auf einem infizierten System verschlüsselt und anschließend ein Lösegeld (englisch: *ransom*) für die Entschlüsselung fordert.

**Typischer Ablauf:**
1. Infektion über [[Phishing]]-E-Mail, bösartige Makros, kompromittierte Webseiten oder ungepatchte Schwachstellen
2. Ausbreitung im Netzwerk (lateral movement)
3. Verschlüsselung aller erreichbaren Dateien (lokal und Netzlaufwerke)
4. Anzeige einer Lösegeldforderung

**Schutzmaßnahmen:**
- Regelmäßige, offline gespeicherte Datensicherungen
- Einschränkung der [[PowerShell ExecutionPolicy]]
- Deaktivierung von Makros in Office ([[Group Policy Object|GPO]])
- Aktueller Virenschutz und Patch-Management
- Netzwerksegmentierung

**Im IT-Grundschutz:** Relevant für SYS.2.1.A6 (Schadsoftwareschutz) und APP.6.A4 (sichere Konfiguration).
