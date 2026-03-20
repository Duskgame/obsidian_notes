---
aliases:
  - CSP
  - Content-Security-Policy
---
[W3C – Content Security Policy Level 3](https://www.w3.org/TR/CSP3/)

Eine **Content Security Policy (CSP)** ist ein Sicherheitsmechanismus im Webbrowser, der über einen HTTP-Header festlegt, welche Inhaltsquellen (Skripte, Styles, Bilder, etc.) eine Webseite laden und ausführen darf.

**Funktionsweise:**
- Der Webserver sendet einen `Content-Security-Policy`-Header mit der Antwort
- Der Browser erlaubt nur Ressourcen aus den explizit erlaubten Quellen
- Nicht autorisierte Inhalte (z. B. injizierte Skripte) werden blockiert

**Schutz vor:**
- **Cross-Site Scripting (XSS):** Eingeschleuste Skripte können nicht ausgeführt werden, wenn ihre Quelle nicht erlaubt ist
- **Clickjacking:** Über `frame-ancestors`-Direktive steuerbar
- **Mixed Content:** Erzwingt HTTPS für alle Ressourcen

**CSP-Level:**
- **CSP Level 1:** Grundlegende Quellenrichtlinien
- **CSP Level 2:** Nonces und Hashes für inline-Skripte
- **CSP Level 3:** Striktere Direktiven, bessere Unterstützung für dynamische Inhalte (aktueller Standard)

**Im IT-Grundschutz:** APP.1.2.A1 fordert, dass eingesetzte Webbrowser die Content Security Policy umsetzen und idealerweise den aktuell höchsten CSP-Level erfüllen.
