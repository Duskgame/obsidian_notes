---
aliases:
  - Sandbox
  - Sandboxed
  - Isolierte Umgebung
---
> **Quelle:** [BSI – Bundesamt für Sicherheit in der Informationstechnik](https://www.bsi.bund.de/)

**Sandboxing** bezeichnet eine Sicherheitstechnik, bei der Prozesse oder Anwendungen in einer isolierten Umgebung (*Sandbox*) ausgeführt werden, die keinen oder nur eingeschränkten Zugriff auf das restliche System hat.

**Prinzip:**
- Ein Prozess in der Sandbox ist strikt von anderen Prozessen und Systemressourcen getrennt
- Schadsoftware, die in einer Sandbox ausgeführt wird, kann das Hostsystem nicht beschädigen
- Auch bei einer Kompromittierung bleibt der Schaden auf die Sandbox beschränkt

**Einsatz in Webbrowsern:**
- Jede Browser-Tab läuft in einer eigenen Sandbox — ein kompromittierter Tab kann nicht auf andere Tabs oder das System zugreifen
- Erweiterungen und Plugins werden ebenfalls sandboxed ausgeführt
- Google Chrome setzt seit Jahren auf Multi-Prozess-Architektur mit Sandboxing als Kernmerkmal

**Weitere Einsatzbereiche:**
- Antivirensoftware testet verdächtige Dateien in einer Sandbox
- Betriebssysteme isolieren Anwendungen (z. B. Windows App Container, macOS App Sandbox)
- Browser-Plugins wie früher Adobe Flash liefen in einer eigenen Sandbox

**Im IT-Grundschutz:** APP.1.2.A1 fordert, dass Webbrowser Sandboxing einsetzen, sodass jede Instanz und jeder Verarbeitungsprozess nur auf die eigenen Ressourcen zugreifen kann.
