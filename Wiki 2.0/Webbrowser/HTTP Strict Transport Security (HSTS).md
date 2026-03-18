---
aliases:
  - " "
  - HSTS
---
https://www.ssldragon.com/de/blog/was-ist-hsts/

**HTTP Strict Transport Security (HSTS)** ist eine web security policy die Browser anweist, sich ausschließlich über HTTPS mit Websites zu verbinden. Durch die Erzwingung sicherer Verbindungen schützt HSTS die Nutzer vor Bedrohungen wie Man-in-the-Middle-Angriffen, SSL-Stripping und Cookie-Hijacking.

Vor HSTS konnte es vorkommen, dass User Agents (Webbrowser), selbst wenn eine Website HTTPS aktiviert hatte, zunächst versuchten, eine Verbindung über ungesichertes **HTTP** herzustellen, was zu Sicherheitslücken führte. Angreifer konnten diese Momente ausnutzen, um den Datenverkehr abzufangen, Benutzer auf bösartige Websites umzuleiten oder [sensible Daten zu stehlen](https://www.ssldragon.com/de/blog/sensible-daten-schutzen/). HSTS behebt dieses Problem, indem es die Browser über einen speziellen Header, **Strict-Transport-Security**, anweist, nach dem ersten sicheren Besuch niemals eine ungesicherte Verbindung zu versuchen.

Durch die Implementierung von HSTS wird die Sicherheit Ihrer Website verbessert, indem unsichere Weiterleitungen eliminiert und Schwachstellen minimiert werden. Dies ist von entscheidender Bedeutung für Websites, die mit sensiblen Benutzerdaten oder Finanztransaktionen umgehen oder die Einhaltung gesetzlicher Vorschriften erfordern.

Es wurde 2012 mit RFC 6797 offiziell eingeführt und enthält klare Richtlinien für seine Anwendung.