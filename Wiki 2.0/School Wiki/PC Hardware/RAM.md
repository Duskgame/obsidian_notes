# RAM — Random Access Memory

[Wikipedia — DIMM](https://en.wikipedia.org/wiki/DIMM) | [Crucial — RAM Guide](https://www.crucial.com/articles/about-memory/support-what-does-ram-stand-for)

RAM is the short-term working memory of a PC. It temporarily stores data the CPU is actively using.

---

## Form Factors

| Module | Use | Notes |
|---|---|---|
| **DIMM** | Desktop Tower | 64-bit bus, standard since late 90s |
| **SODIMM** | Laptops, Mini-PCs | Smaller, "Small Outline DIMM" |
| **SIMM** | Legacy (80s/90s) | 32-bit bus, obsolete |
| **DIP** | Very old systems | Individual chips, not modules |

> Rule: **Tower PC = DIMM**, **Laptop = SODIMM**

---

## Generations

| Type | Typical Speed | Voltage |
|---|---|---|
| DDR4 | 2133–3200 MHz | 1.2V |
| DDR5 | 4800–6400 MHz | 1.1V |

Generation must match the motherboard — DDR4 and DDR5 slots are not interchangeable.

---

## Buffered / ECC RAM

**Buffered (Registered) RAM** has an extra register chip between the memory controller and chips. Used in **servers and workstations** because it:
- Supports more modules per system (hundreds of GB)
- Offers greater signal stability
- Often includes **ECC** (Error Correcting Code) for data integrity

Not compatible with standard desktop motherboards.

---

## Correct Installation

- RAM slots have a **notch (Kerbe)** that matches a key on the module — it can only be inserted one way
- The notch prevents incorrect orientation and damage
- Slot colors indicate **Dual Channel pairs** (not insertion direction)

---

## Replacing RAM — two key factors

1. **Motherboard compatibility** — correct generation (DDR4/DDR5), form factor (DIMM/SODIMM), voltage
2. **Chipset-supported speed** — the board's chipset defines the maximum supported frequency; faster RAM will run at the board's max speed

---

## Related

- [[Motherboard]] — RAM slots and chipset
- [[CPU]] — memory controller is built into modern CPUs
