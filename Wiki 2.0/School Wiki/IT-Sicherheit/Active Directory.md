---
aliases:
  - AD
  - Active Directory Domain Services
  - AD DS
---
**Active Directory (AD)** ist ein Verzeichnisdienst von Microsoft, der in Windows-Domänennetzwerken zur zentralen Verwaltung von Benutzern, Computern, Gruppen und Ressourcen eingesetzt wird.

**Kernfunktionen:**
- **Authentisierung:** Zentrale Anmeldung aller Domänenmitglieder (Single Sign-On)
- **Autorisierung:** Verwaltung von Zugriffsrechten auf Ressourcen
- **[[Group Policy Object|Gruppenrichtlinien (GPO)]]:** Zentrale Konfiguration von Sicherheitseinstellungen für alle Domänencomputer
- **Zertifikatsverwaltung:** Hinterlegung von [[BitLocker]]-Recovery-Keys

**Relevanz für C023:**
- Client C023 ist Mitglied der Windows-Domäne
- Profile werden auf dem Dateiserver gespeichert (Domänenrichtlinie)
- Softwareverteilung und Sicherheitskonfiguration über GPO

**Im IT-Grundschutz:** SYS.2.2.3.A6 fordert, dass die Anmeldung nur über einen selbst betriebenen Verzeichnisdienst (wie Active Directory) möglich ist.
