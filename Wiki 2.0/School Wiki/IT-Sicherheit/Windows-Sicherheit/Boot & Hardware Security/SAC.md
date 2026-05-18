---
aliases:
  - Semi-Annual Channel
  - Current Branch for Business
  - CBB
  - Windows Update-Kanal
---
[Microsoft Learn – Windows as a Service (WaaS)](https://learn.microsoft.com/de-de/windows/deployment/update/waas-overview)

Der **Semi-Annual Channel (SAC)** ist ein Windows-Update-Modell, bei dem Microsoft zweimal jährlich (im Frühjahr und Herbst) ein größeres Feature-Update für Windows 10/11 veröffentlicht.

**Windows-Servicing-Kanäle im Überblick:**

| Kanal | Zielgruppe | Feature-Updates | Typische Verzögerung |
|---|---|---|---|
| **SAC** (Semi-Annual Channel) | Unternehmen | 2× jährlich | Bis zu 18 Monate wählbar |
| **LTSC** (Long-Term Servicing Channel) | Spezialsysteme | Alle 2–3 Jahre | – |

**Relevanz für Sicherheit:**
- Unkonfigurierter Update-Kanal führt zu unkontrollierten Feature-Updates
- Maximale Update-Verzögerung (wie bei C023) bedeutet bekannte Sicherheitslücken bleiben lange ungepatcht
- BSI empfiehlt, den Update-Kanal explizit per [[Group Policy Object|GPO]] festzulegen und Sicherheitsupdates innerhalb von 30 Tagen einzuspielen

**Im IT-Grundschutz:** SYS.2.2.3.A2 fordert die Festlegung eines geeigneten Lizenzmodells und Service-Branch.
