**Site Isolation** ist eine Sicherheitsfunktion, die Webseiten strikt voneinander trennt.

Normalerweise lädt Chrome verschiedene Webseiten in sogenannten „Renderer-Prozessen“. Ohne Site Isolation können mehrere Webseiten in einem einzigen Prozess laufen.

Mit Site Isolation bekommt jede Webseite (bzw. jede Domain) ihren eigenen Prozess.

Dadurch wird verhindert, dass bösartiger Code einer Webseite auf Daten anderer Webseiten zugreifen kann, die im selben Browser geöffnet sind.