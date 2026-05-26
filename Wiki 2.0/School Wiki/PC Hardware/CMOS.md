# CMOS

[Wikipedia – CMOS](https://en.wikipedia.org/wiki/Nonvolatile_BIOS_memory) | [Computer Hope – CMOS](https://www.computerhope.com/jargon/c/cmos.htm)

CMOS (Complementary Metal-Oxide-Semiconductor) in the PC context refers to a small region of battery-backed RAM on the [[Motherboard]] that stores [[UEFI]]/BIOS configuration settings. It keeps time and user settings alive when the system is powered off.

---

## What CMOS Stores

- System date and time (RTC — Real-Time Clock)
- Boot device order
- Hardware configuration (CPU speed, RAM timings, virtualization toggle)
- UEFI/BIOS passwords
- Enabled/disabled peripherals (onboard audio, USB ports, etc.)

CMOS is the *settings file* for [[UEFI]]. The firmware code itself lives on the [[ROM Chip]] (flash); CMOS only holds the user's customizations.

---

## The CMOS Battery

A coin cell battery (typically **CR2032**, 3V) on the motherboard continuously powers the CMOS chip, even when the PC is unplugged.

```
Motherboard
├── Flash chip  →  UEFI firmware binary (permanent)
└── CMOS chip   →  UEFI settings + RTC  (battery-backed)
         ↑
    CR2032 coin cell
    (~3–10 year lifespan)
```

**Dead battery symptoms:**
- Clock resets to a default date/time on every boot
- BIOS settings revert to defaults after power loss
- POST warning: "CMOS checksum error" or "RTC battery low"

---

## Clearing CMOS

Clearing CMOS resets all UEFI/BIOS settings to factory defaults. Common reasons:

- Bad overclock settings prevent the PC from booting
- Forgotten BIOS password
- Troubleshooting POST failures

**Methods:**

| Method | Steps |
|--------|-------|
| Remove battery | Power off, unplug, remove CR2032 for 30–60 seconds, reinsert |
| Clear CMOS jumper | Move the 3-pin jumper on the motherboard to the CLR_CMOS position for a few seconds |
| Dedicated button | Some motherboards have a physical "Clear CMOS" button on the rear I/O panel |

After clearing, the system will prompt to re-enter the date/time and any custom BIOS settings.

---

## CMOS vs. ROM Chip

| | [[ROM Chip]] (Flash) | CMOS |
|---|---|---|
| Stores | Firmware binary (UEFI code) | User settings + RTC |
| Volatile? | No — survives without power | No — needs battery |
| Writable? | Yes, but rarely (firmware updates) | Yes, changed in BIOS UI |
| Reset by clearing CMOS? | No | Yes |

---

## Related Topics

- [[UEFI]] — firmware whose settings are saved in CMOS
- [[ROM Chip]] — stores the firmware code itself, distinct from CMOS
- [[Motherboard]] — hosts both the CMOS chip and the CR2032 battery socket
- [[Secure Boot]] — setting stored in CMOS; clearing CMOS disables it until re-enabled
- [[TPM]] — another hardware security component initialized alongside UEFI
