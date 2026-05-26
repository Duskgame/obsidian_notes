# ROM Chip

[Wikipedia – ROM](https://en.wikipedia.org/wiki/Read-only_memory) | [Computer Hope – ROM Types](https://www.computerhope.com/jargon/r/rom.htm)

A ROM (Read-Only Memory) chip is a type of non-volatile memory that retains its contents without power. Originally truly read-only (written once at the factory), modern variants are electrically rewritable and serve as the standard medium for storing firmware.

---

## Types of ROM

| Type | Full Name | Erasable? | How |
|------|-----------|-----------|-----|
| ROM | Read-Only Memory | No | Factory-programmed, permanent |
| PROM | Programmable ROM | No | User-programmed once (fuse-burning) |
| EPROM | Erasable PROM | Yes | UV light exposure |
| EEPROM | Electrically Erasable PROM | Yes | Electrical signal, byte by byte |
| Flash | Flash Memory | Yes | Electrical signal, in blocks |

Flash is the dominant modern type — it balances rewritability, density, and speed.

---

## How Firmware Is Stored

The [[UEFI]] / BIOS firmware on a PC is stored on a flash chip soldered directly to the [[Motherboard]]. This is why firmware survives a hard drive wipe or complete [[RAM]] loss — the chip is independent of all other storage.

```
Motherboard
└── Flash chip (SPI NOR Flash)
    └── UEFI firmware binary
        ├── Boot manager
        ├── Hardware init code
        └── UEFI settings variables → CMOS (separate, battery-backed)
```

The chip communicates with the CPU over the SPI (Serial Peripheral Interface) bus.

---

## Characteristics

- **Non-volatile:** data persists without power
- **Fast read, slower write:** optimized for reading (firmware is read far more often than written)
- **Limited write cycles:** flash cells wear out after ~100,000 erase cycles
- **Block erase:** flash must erase an entire block before rewriting individual bytes (unlike EEPROM)

---

## Common Uses

- PC/server firmware ([[UEFI]], BMC, NIC firmware)
- Microcontroller program storage (embedded systems)
- Game cartridges (retro consoles stored the entire game in ROM)
- [[Secure Boot]] certificate storage
- [[TPM]] initialization code

---

## Related Topics

- [[UEFI]] — firmware stored on the flash chip; reads from it at every boot
- [[Motherboard]] — hosts the flash chip on the SPI bus
- [[Secure Boot]] — relies on keys stored in the flash chip
- [[TPM]] — separate chip, but initialized alongside firmware from flash
- [[Storage Interfaces]] — contrast with mass storage (SSD/HDD use NAND flash at a higher level)
