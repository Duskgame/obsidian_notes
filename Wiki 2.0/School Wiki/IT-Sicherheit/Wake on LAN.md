---
aliases:
  - WoL
  - WakeOnLAN
---
**Wake on LAN (WoL)** ist eine Netzwerkfunktion, die es ermöglicht, einen ausgeschalteten oder im Standby befindlichen Computer durch ein spezielles Netzwerkpaket (sogenanntes *Magic Packet*) aus der Ferne einzuschalten.

**Funktionsweise:**
- Die Netzwerkkarte (NIC) bleibt auch bei ausgeschaltetem PC mit Strom versorgt
- Beim Empfang eines Magic Packets (enthält die MAC-Adresse des Zielgeräts) startet das System
- Konfiguration im [[UEFI]]

**Einsatzzwecke:**
- Fernwartung und zentrale Softwareverteilung (z. B. Patches einspielen außerhalb der Arbeitszeiten)
- [[WSUS]]-basierte Update-Verteilung nachts

**Sicherheitsrisiko:** Wenn nicht für die IT-Administration benötigt, vergrößert WoL die Angriffsfläche (unberechtigtes Einschalten von außen). Das BSI empfiehlt, nicht benötigte Firmwarefunktionen zu deaktivieren (SYS.2.1.A8).
