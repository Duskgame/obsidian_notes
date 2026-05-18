# Motherboard

[CompTIA A+ Reference](https://www.comptia.org/certifications/a) | [PC Part Picker — Motherboard Guide](https://pcpartpicker.com/learn/guide/choosing-a-motherboard)

The motherboard (mainboard) is the central circuit board connecting all PC components. It determines what RAM, CPU, and expansion cards are compatible.

---

## Form Factors

The form factor defines the physical size and mounting hole positions — it must match the case.

| Form Factor | Size | Typical Use |
|---|---|---|
| **ATX** | 305 × 244 mm | Full/Mid-Tower Desktop |
| **Micro-ATX** | 244 × 244 mm | Mid/Mini-Tower Desktop |
| **Mini-ITX** | 170 × 170 mm | Small Form Factor builds |

> The motherboard form factor is the most important factor when selecting a **case and PSU**.

---

## System Bus

The **system bus** (Frontside Bus / FSB) connects the CPU to RAM and other motherboard components. In modern CPUs the memory controller is built directly into the CPU:

- **Intel:** DMI (Direct Media Interface)
- **AMD:** Infinity Fabric

---

## Expansion Slots

### PCIe (PCI Express) — current standard
Serial bus — transmits data bit by bit per lane.

| Slot | Lanes | Length | Typical Use |
|---|---|---|---|
| **PCIe x1** | 1 | very short | Sound, WLAN cards |
| **PCIe x4** | 4 | medium | SSD controllers |
| **PCIe x8** | 8 | long | RAID, network cards |
| **PCIe x16** | 16 | very long | **Graphics cards** |

### PCI — legacy (parallel bus, obsolete)
### AGP — legacy (graphics only, obsolete)

---

## I/O Shield

A metal plate that ships with the motherboard and is pressed into the rear of the case. It exposes only the specific rear ports of that board (USB, HDMI, LAN, Audio). Required because every motherboard has a different port layout.

---

## Standoffs (Abstandshalter)

Small brass bolts screwed into the case before mounting the motherboard. They lift the board off the metal chassis to prevent **short circuits**. The standoff pattern must match the motherboard form factor.

---

## Front Panel Connectors

Thin cables connecting power button, reset button, HDD LED, and power LED to the motherboard header. **Pin 1** is identified by a small arrow or notch on the connector. LEDs have polarity: colored wire = +, white/black = −.

---

## Related

- [[RAM]] — memory slots on the motherboard
- [[CPU]] — processor socket types
- [[PSU]] — power supply compatibility
- [[Storage Interfaces]] — SATA connectors on the board
