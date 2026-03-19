---
aliases:
  - Site-Isolation
  - Cross-Site-Isolation
---
> **Quelle:** [Chromium – Site Isolation](https://www.chromium.org/Home/chromium-security/site-isolation/)

**Site Isolation** ist eine Sicherheitsarchitektur in Webbrowsern (insbesondere Google Chrome), bei der jede Website in einem eigenen dedizierten Prozess ausgeführt wird.

**Hintergrund:**
- Klassische Browser nutzten einen gemeinsamen Renderer-Prozess für mehrere Tabs
- Dies ermöglichte Angriffe wie **Spectre/Meltdown** (Seitenkanal-Angriffe), bei denen ein kompromittierter Tab auf den Speicher anderer Tabs zugreifen konnte

**Funktionsweise:**
- Jede *Site* (Domain inkl. Subdomains) erhält einen eigenen Betriebssystemprozess
- Cross-site Ressourcen (Iframes, Skripte von fremden Domains) werden in separaten Prozessen gerendert
- Der Betriebssystem-Kernel schützt die Prozessgrenzen — selbst bei einem Renderer-Exploit kann kein Speicher anderer Prozesse gelesen werden

**Abgrenzung zu [[Sandboxing]]:**
- Sandboxing isoliert einen Prozess vom System
- Site Isolation trennt Websites voneinander auf Prozessebene

**Im IT-Grundschutz:** APP.1.2.A1 fordert, dass Webseiten als eigenständige Prozesse oder mindestens als eigene Threads voneinander isoliert werden.
