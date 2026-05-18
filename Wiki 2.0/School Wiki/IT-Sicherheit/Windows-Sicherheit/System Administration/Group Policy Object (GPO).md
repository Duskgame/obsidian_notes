---
aliases:
  - GPO
  - Gruppenrichtlinien
  - Group Policy
  - Group Policy Objects
  - Domänen-GPO
---
[Microsoft Learn – Gruppenrichtlinien](https://learn.microsoft.com/de-de/windows-server/identity/ad-ds/manage/group-policy/group-policy-overview)

Ein **Group Policy Object (GPO)** ist ein zentrales Konfigurationsobjekt in Windows-Domänen, das Sicherheits- und Konfigurationseinstellungen für Benutzer und Computer automatisch durchsetzt.

**Funktionsweise:**
- GPOs werden im **[[Active Directory]]** erstellt und an Organisations-Einheiten (OUs), Domains oder Sites verknüpft
- Beim Anmelden oder Starten wendet Windows die geltenden GPOs automatisch an
- Einstellungen werden zentral vom Administrator verwaltet — Benutzer können sie nicht überschreiben

**Typische Einsatzgebiete:**
- Sicherheitseinstellungen: Passwortrichtlinien, Bildschirmsperre, Firewall-Konfiguration
- Software-Deployment: Automatisches Installieren/Deinstallieren von Software
- Skript-Ausführung: Logon-/Logoff-Skripte
- Windows Update-Konfiguration: [[WSUS]]-Server, Update-Verzögerungen
- Browser-Konfiguration: Chrome Enterprise Policies, [[PowerShell ExecutionPolicy]]
- Einschränkungen: Blockieren externer Medien, Deaktivieren von Cloud-Funktionen

**Administrative Templates (ADMX):**
- Hersteller wie Microsoft oder Google stellen ADMX-Vorlagen bereit, die die GPO-Einstellungen ihrer Produkte beschreiben
- Beispiel: Google Chrome ADMX ermöglicht die Konfiguration von Chrome-Sicherheitseinstellungen per GPO

**Im IT-Grundschutz:** Viele BSI-Anforderungen (z. B. SYS.2.1.A1, APP.6.A4) werden in Unternehmensumgebungen über GPOs umgesetzt.
