---
aliases:
  - Trusted Platform Module
  - TPM 2.0
---
Ein **Trusted Platform Module (TPM)** ist ein dedizierter Sicherheitschip (oder eine Firmware-Komponente) auf einem Mainboard, der kryptografische Schlüssel sicher speichert und grundlegende Sicherheitsfunktionen bereitstellt.

**Funktionen:**
- Speichert kryptografische Schlüssel, Zertifikate und Passwort-Hashes hardwaregeschützt
- Ermöglicht die Messung des Systemzustands beim Booten (Platform Integrity)
- Grundlage für [[BitLocker]]-Festplattenverschlüsselung unter Windows
- Voraussetzung für [[Secure Boot]]-Attestierung

**Versionen:** TPM 1.2 (veraltet) und TPM 2.0 (aktueller Standard, Voraussetzung für Windows 11).

**Im IT-Grundschutz:** SYS.2.1.A8 fordert die kryptografische Absicherung des Bootvorgangs – das TPM ist hierfür die technische Grundlage.
