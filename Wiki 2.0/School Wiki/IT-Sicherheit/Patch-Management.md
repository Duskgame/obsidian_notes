---
aliases:
  - Patch Management
  - Patchmanagement
  - Patch-Prozess
  - Softwareaktualisierung
---
> **Quelle:** [BSI – IT-Grundschutz-Kompendium SYS.2.1](https://www.bsi.bund.de/DE/Themen/Unternehmen-und-Organisationen/Standards-und-Zertifizierung/IT-Grundschutz/IT-Grundschutz-Kompendium/it-grundschutz-kompendium_node.html)

**Patch-Management** bezeichnet den strukturierten Prozess zur Identifikation, Bewertung, Priorisierung und Installation von Software-Updates (Patches) in einer IT-Infrastruktur.

**Ziele:**
- Schließen bekannter Sicherheitslücken (Schwachstellen-Management)
- Sicherstellung der Systemstabilität und -funktionalität
- Einhaltung von Compliance-Anforderungen

**Prozessschritte:**
1. **Inventarisierung:** Erfassung aller eingesetzten Software und Versionen
2. **Monitoring:** Überwachung auf neue Patches und Sicherheitsmeldungen (CVEs)
3. **Bewertung:** Priorisierung nach Kritikalität (z. B. CVSS-Score)
4. **Test:** Patches in einer Testumgebung prüfen, bevor sie produktiv eingespielt werden
5. **Deployment:** Verteilung der Patches, ggf. über zentrale Systeme wie [[WSUS]]
6. **Verifikation:** Kontrolle, ob Patches korrekt installiert wurden

**Zentrale Verteilung:**
- **[[WSUS]]** (Windows Server Update Services): Microsoft-Lösung zur zentralen Windows-Update-Verwaltung
- GPO-gesteuerte Softwareverteilung für Unternehmensumgebungen

**Im IT-Grundschutz:** SYS.2.1.A3 fordert aktivierte Autoupdate-Mechanismen oder ein zentrales Patch-Management-System. APP.6.A4 fordert, dass alle relevanten Sicherheitsupdates installiert sein müssen, bevor Software produktiv eingesetzt wird.
