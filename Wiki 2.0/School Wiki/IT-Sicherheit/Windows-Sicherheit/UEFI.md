---
aliases:
  - Unified Extensible Firmware Interface
  - BIOS
---
[UEFI Forum – Spezifikationen](https://uefi.org/specifications)

**UEFI (Unified Extensible Firmware Interface)** ist der Nachfolger des klassischen BIOS und dient als Schnittstelle zwischen Hardware und Betriebssystem beim Startvorgang eines Computers.

**Sicherheitsrelevante Funktionen:**
- **Administrator-Passwort:** Schützt die UEFI-Einstellungen vor unbefugter Änderung
- **User-Passwort:** Verhindert das Starten des Systems ohne Authentisierung
- **[[Secure Boot]]:** Erlaubt nur das Starten signierter Bootloader und Betriebssysteme
- **Boot-Reihenfolge:** Legt fest, von welchen Medien (Festplatte, USB, Netzwerk) gestartet werden darf
- **[[TPM]]-Verwaltung:** Aktivierung/Deaktivierung des Sicherheitschips

**Im IT-Grundschutz:** SYS.2.1.A8 fordert die Absicherung des Bootvorgangs durch UEFI-Passwörter, Einschränkung der Boot-Medien und Deaktivierung nicht benötigter Firmwarefunktionen.
