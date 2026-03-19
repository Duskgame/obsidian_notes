---
aliases:
  - BitLocker Drive Encryption
---
**BitLocker** ist die in Windows integrierte Festplattenverschlüsselungslösung von Microsoft. Sie verschlüsselt ganze Laufwerke (Volumes) und schützt so Daten vor unbefugtem Zugriff, insbesondere bei Diebstahl oder Verlust des Geräts.

**Funktionsweise:**
- Verschlüsselt das gesamte Laufwerk mit AES (128 oder 256 Bit)
- Nutzt idealerweise das [[TPM]] zur sicheren Schlüsselspeicherung
- Beim Booten prüft das TPM die Systemintegrität; nur bei unverändertem System wird der Schlüssel freigegeben
- Recovery Key kann in [[Active Directory]] oder Microsoft-Konto hinterlegt werden

**Voraussetzungen:**
- [[TPM]] 1.2 oder 2.0 (empfohlen: 2.0)
- [[UEFI]] mit [[Secure Boot]] (empfohlen)
- Windows Pro, Enterprise oder Education

**Im IT-Grundschutz:** SYS.2.1.A8 empfiehlt kryptografischen Schutz des Bootvorgangs; BitLocker ist die Standardlösung hierfür unter Windows.
