---
aliases:
  - Telemetrie-Daten
  - Diagnosedaten
  - Nutzungsdaten
  - Diagnose- und Nutzungsdaten
---
**Telemetrie** bezeichnet im IT-Kontext die automatische Übermittlung von Diagnose-, Nutzungs- und Systemdaten von einem Gerät oder einer Software an den Hersteller, um Fehler zu analysieren, die Produktentwicklung zu unterstützen oder den Betrieb zu überwachen.

**Datenschutzrelevanz:**
- Telemetriedaten können personenbezogene Informationen enthalten (Nutzungsverhalten, Gerätekennungen, Standortdaten)
- Übertragung an externe Server (z. B. Microsoft, Google) → Datenschutzrechtlich relevant (DSGVO)
- Minimierung von Telemetriedaten ist ein Gebot der **Datensparsamkeit**

**Windows-Telemetrie-Level:**
| Level | Name | Verfügbar in | Beschreibung |
|-------|------|-------------|--------------|
| 0 | Security | Enterprise/Education | Minimale Diagnosedaten; nur sicherheitsrelevante Daten |
| 1 | Basic | alle Editionen | Grundlegende Gerätedaten |
| 2 | Enhanced | alle Editionen | Erweiterte Nutzungsdaten |
| 3 | Full | alle Editionen | Vollständige Diagnose- und Absturzdaten |

**Hinweis:** Telemetrie-Level 0 ist nur in der **Enterprise- und Education-Edition** verfügbar. In der **Pro-Edition** ist Level 1 das Minimum.

**Im IT-Grundschutz:** SYS.2.2.3.A4 fordert, das Telemetrie-Level 0 in der Enterprise-Edition zu konfigurieren oder alternativ die Übertragung auf Netzebene zu unterbinden. APP.6.A4 fordert datensparsamste Einstellungen für Software.
