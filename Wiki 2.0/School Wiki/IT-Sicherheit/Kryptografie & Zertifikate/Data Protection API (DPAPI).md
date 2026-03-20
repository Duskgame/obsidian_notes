---
aliases:
  - DPAPI
  - Data Protection API
  - Windows DPAPI
---
[Microsoft Learn – Data Protection API](https://learn.microsoft.com/en-us/windows/win32/seccng/cng-dpapi)

**DPAPI (Data Protection API)** ist eine Windows-Komponente zur transparenten Verschlüsselung sensibler Daten auf Benutzer- oder Maschinenebene. Browser wie Google Chrome und Microsoft Edge nutzen DPAPI, um gespeicherte Passwörter, Cookies und Token zu schützen.

**Prinzip:**
- Anwendungen rufen `CryptProtectData()` auf, um Daten zu verschlüsseln
- Windows leitet den Verschlüsselungsschlüssel aus dem Anmelde-Passwort des Benutzers ab
- Entschlüsselung (`CryptUnprotectData()`) ist nur unter demselben Benutzerkonto möglich
- Kein manuelles Schlüsselmanagement nötig

**Schutzebenen:**

| Ebene | Binding | Nutzung |
|-------|---------|---------|
| Benutzer | Angemeldeter Benutzer | Browser-Passwörter, Zertifikate |
| Maschine | Lokales Gerät | Dienst-Credentials, IIS-Schlüssel |

**Einsatz in Webbrowsern:**
- Chrome/Edge verschlüsseln `Login Data`-SQLite-Datei mit DPAPI
- Schützt vor Offline-Angriff auf den Passwort-Speicher (Datei ohne Benutzerkontext nicht nutzbar)
- Seit Chrome 127: zusätzliche App-Bound-Encryption als Schutz gegen Infostealer

**Im IT-Grundschutz:** APP.1.2.A5 fordert den Schutz lokal gespeicherter Zugangsdaten — DPAPI ist die Windows-native Umsetzung dieses Schutzes.
