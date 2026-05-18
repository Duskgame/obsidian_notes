# CPU — Central Processing Unit

[Intel — CPU Guide](https://www.intel.com/content/www/us/en/products/docs/processors/what-is-a-cpu.html) | [AMD — Processor Guide](https://www.amd.com/en/processors)

---

## Package Types (Socket)

| Type | Pins location | Used by |
|---|---|---|
| **LGA** (Land Grid Array) | Pins in the **socket** on the motherboard | Intel |
| **PGA** (Pin Grid Array) | Pins on the **processor** itself | AMD (AM4 and older) |
| **BGA** (Ball Grid Array) | Soldered directly to board | Laptops (not replaceable) |

> LGA = pins in socket (Intel), PGA = pins on chip (AMD)

---

## CPU Installation — Key Steps

1. **Antistatik precautions** — use ESD wrist strap or touch grounded metal before handling
2. **Correct orientation** — align the triangle/notch marker on the CPU with the socket marker; wrong orientation damages pins
3. **Zero Insertion Force (ZIF)** — lift the lever, place CPU without force, lower lever to lock. Never press hard.
4. **Apply thermal paste** — small pea-sized amount on the CPU center before mounting the cooler
5. **Mount the cooler** — CPU overheats and is permanently damaged within seconds without a functioning cooler

---

## Cooling

Without a cooler the CPU hits thermal limits in seconds. A **heatsink + fan (HSF)** unit draws heat away from the CPU die. Thermal paste fills microscopic air gaps between the CPU and heatsink surface.

---

## Related

- [[Motherboard]] — CPU socket, chipset, FSB/DMI
- [[RAM]] — memory controller is inside the CPU in modern systems
- [[PSU]] — CPU TDP contributes to total power draw
