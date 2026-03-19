---
aliases:
  - ExecutionPolicy
  - PowerShell-Ausführungsrichtlinie
---
Die **PowerShell ExecutionPolicy** (Ausführungsrichtlinie) legt fest, welche PowerShell-Skripte auf einem Windows-System ausgeführt werden dürfen. Sie ist eine Sicherheitsmaßnahme, die das versehentliche oder böswillige Ausführen nicht autorisierter Skripte verhindern soll.

**Stufen (von unsicher zu sicher):**

| Stufe | Bedeutung |
|---|---|
| `Unrestricted` | Alle Skripte werden ausgeführt (nur Warnung bei nicht vertrauenswürdigen) |
| `RemoteSigned` | Lokale Skripte laufen frei; Skripte aus dem Internet müssen signiert sein |
| `AllSigned` | Alle Skripte müssen digital signiert sein |
| `Restricted` | Keine Skripte erlaubt (nur interaktive Eingaben) |

**Empfehlung:** Mindestens `RemoteSigned` per [[Group Policy Object|GPO]] setzen. `Unrestricted` stellt ein kritisches Sicherheitsrisiko dar, da [[Ransomware]] und andere Schadsoftware PowerShell häufig als Angriffsvektor nutzen.

**Im IT-Grundschutz:** APP.6.A4 fordert, dass Software mit den geringsten notwendigen Berechtigungen und in sicherer Konfiguration ausgeführt wird.
