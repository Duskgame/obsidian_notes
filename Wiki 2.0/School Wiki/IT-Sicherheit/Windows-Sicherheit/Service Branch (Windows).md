---
aliases:
  - Service Branch
  - CB
  - CBB
  - LTSC
  - Windows Update-Kanal
  - Current Branch
  - Current Branch for Business
  - Long-Term Servicing Channel
---
> **Quelle:** [Microsoft Learn – Windows as a Service (WaaS)](https://learn.microsoft.com/de-de/windows/deployment/update/waas-overview)

**Service Branch** bezeichnet den Update-Kanal, über den ein Windows-System Feature-Updates erhält. Microsoft bietet verschiedene Kanäle mit unterschiedlicher Update-Frequenz und Unterstützungsdauer.

**Übersicht der Service Branches:**

| Branch | Abkürzung | Zielgruppe | Feature-Updates | Unterstützungsdauer |
|--------|-----------|------------|-----------------|---------------------|
| **Current Branch** | CB | Privatnutzer, Early Adopter | Sofort nach Release | ~18 Monate |
| **Current Branch for Business** | CBB | Unternehmen | Mit Verzögerung (ca. 4 Monate) | ~18 Monate |
| **Long-Term Servicing Channel** | LTSC | kritische Systeme, Kiosk | Keine Feature-Updates | 10 Jahre |

**Sicherheitsrelevanz:**
- Alle Branches erhalten **Sicherheitsupdates** gleich schnell (Patch Tuesday)
- LTSC erhält **keine neuen Features** — ideal für stabile, unveränderliche Systeme (z. B. Industrieanlagen, Geldautomaten)
- CBB ermöglicht Unternehmen, Feature-Updates in einer Testphase zu evaluieren, bevor sie ausgerollt werden

**Hinweis:** Die Begriffe haben sich mit Windows 10 verändert. Neuere Bezeichnungen sind *General Availability Channel* (früher CB/CBB) und *Long-Term Servicing Channel* (LTSC).

**Im IT-Grundschutz:** SYS.2.2.3.A2 fordert, dass der geeignete Service Branch (CB, CBB oder LTSC) entsprechend des Schutzbedarfs und Einsatzzwecks ausgewählt und dokumentiert wird.
