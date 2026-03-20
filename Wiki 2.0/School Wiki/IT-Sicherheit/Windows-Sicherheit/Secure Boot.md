---
aliases:
  - SecureBoot
---
[Microsoft Learn – Secure Boot](https://learn.microsoft.com/de-de/windows-hardware/design/device-experiences/oem-secure-boot)

**Secure Boot** ist ein [[UEFI]]-Sicherheitsmechanismus, der sicherstellt, dass beim Systemstart nur digital signierte und vertrauenswürdige Software (Bootloader, Betriebssystem-Kernel) ausgeführt wird.

**Funktionsweise:**
- Das [[UEFI]] prüft die digitale Signatur jeder geladenen Komponente anhand einer gespeicherten Datenbank vertrauenswürdiger Schlüssel (Signature Database)
- Nicht signierte oder manipulierte Komponenten werden blockiert
- Verhindert das Starten von Rootkits, Bootkits und nicht autorisierten Betriebssystemen

**Voraussetzungen:**
- [[UEFI]]-Firmware (kein klassisches BIOS)
- Betriebssystem mit gültiger Signatur (Windows 8+ und aktuelle Linux-Distributionen)

**Im IT-Grundschutz:** Teil der Bootvorgang-Absicherung nach SYS.2.1.A8.
