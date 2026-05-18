# Storage Interfaces & Drive Bays

[Wikipedia — SATA](https://en.wikipedia.org/wiki/SATA) | [Wikipedia — Drive bay](https://en.wikipedia.org/wiki/Drive_bay)

---

## Interface Evolution

| Interface | Type | Status | Notes |
|---|---|---|---|
| **IDE / PATA** | Parallel | Obsolete | Wide ribbon cable, max 2 devices per cable |
| **EIDE** | Parallel | Obsolete | Extended IDE, slightly faster |
| **SATA** | Serial | **Current standard** | Thin cable, hot-swap, up to 600 MB/s (SATA III) |
| **NVMe (M.2)** | Serial (PCIe) | Current (SSDs) | Much faster than SATA, directly on motherboard |

> For a new mechanical HDD on a modern motherboard: always recommend **SATA**.

---

## SATA HDD Form Factors

| Size | Use |
|---|---|
| **3.5 inch** | Desktop HDDs — larger capacity, cheaper |
| **2.5 inch** | Laptop HDDs and SSDs — compact |

---

## Drive Bays

| Bay Size | Used For |
|---|---|
| **5.25 inch** | Optical drives (CD/DVD/Blu-ray) |
| **3.5 inch** | Desktop HDDs |
| **2.5 inch** | SSDs, Laptop HDDs |

> 5.25-inch bays are increasingly absent from modern cases as optical drives are rarely used.

---

## RAID

RAID is not an interface — it is a **drive redundancy/performance strategy** (e.g. RAID 1 = mirroring, RAID 5 = striping with parity). RAID runs on top of SATA or NVMe.

---

## Related

- [[Motherboard]] — SATA ports on the board
- [[RAM]] — different from storage; RAM is volatile
