---
aliases:
  - SOP
  - Same-Origin
---
Die **Same-Origin-Policy (SOP)** ist ein grundlegendes Sicherheitsprinzip in Webbrowsern, das verhindert, dass Skripte einer Webseite auf Ressourcen oder Daten einer anderen Webseite zugreifen, wenn diese einen unterschiedlichen Ursprung (*Origin*) haben.

**Definition "Origin":**
Ein Ursprung setzt sich zusammen aus:
- **Protokoll** (z. B. `https`)
- **Domain** (z. B. `example.com`)
- **Port** (z. B. `443`)

Nur wenn alle drei Teile identisch sind, gelten zwei URLs als gleicher Ursprung.

**Schutz vor:**
- **Cross-Site Request Forgery (CSRF):** Verhindert, dass fremde Seiten im Namen des Nutzers Anfragen absetzen
- **Datendiebstahl:** Skripte können keine Cookies, localStorage oder DOM-Inhalte anderer Origins auslesen

**Ausnahmen:**
- CORS (Cross-Origin Resource Sharing) ermöglicht gezielt erlaubte Cross-Origin-Zugriffe
- Bestimmte Ressourcen wie Bilder oder CSS können domainübergreifend eingebunden werden

**Im IT-Grundschutz:** APP.1.2.A1 fordert, dass eingesetzte Webbrowser Maßnahmen zur Same-Origin-Policy unterstützen.
