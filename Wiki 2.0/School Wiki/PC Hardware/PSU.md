# PSU — Power Supply Unit

[Wikipedia — Power supply unit (PC)](https://en.wikipedia.org/wiki/Power_supply_unit_(computer))

The PSU converts AC mains power to the DC voltages needed by PC components (12V, 5V, 3.3V).

---

## Key Selection Factors

When buying or replacing a PSU, two factors matter most:

| Factor | Why |
|---|---|
| **Wattage** | Must supply enough power for all components combined; too little = crashes or no boot |
| **Form factor** | Must physically fit the case (ATX, SFX, etc.) |

---

## Form Factors

| PSU Form Factor | Matching Case |
|---|---|
| **ATX** | Standard Mid/Full-Tower |
| **SFX** | Small Form Factor cases |
| **TFX** | Slim desktop cases |

---

## Output Voltages

All ATX PSUs output standardized voltages — this is not a selection criterion when buying:

| Rail | Used by |
|---|---|
| **12V** | CPU, GPU, motors |
| **5V** | USB, older drives |
| **3.3V** | RAM, chipset |

---

## High-End GPU Considerations

Powerful graphics cards (150–450W) may require:
- A **higher wattage PSU** (often 650–1000W total system)
- **Dedicated PCIe power connectors** (6-pin, 8-pin, or 16-pin directly from PSU)

---

## PSU and Motherboard Form Factor

```
Motherboard form factor → Case size → PSU form factor
CPU + GPU TDP          → Total wattage needed
```

The motherboard is the primary component that determines both case and PSU requirements.

---

## Related

- [[Motherboard]] — form factor drives case/PSU choice
- [[CPU]] — TDP contributes to power budget
