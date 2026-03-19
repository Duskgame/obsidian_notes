---
aliases:
  - SRI
---
> **Quelle:** [W3C – Subresource Integrity](https://www.w3.org/TR/SRI/)

**Subresource Integrity (SRI)** ist ein Sicherheitsmechanismus im Webbrowser, der sicherstellt, dass extern geladene Ressourcen (z. B. JavaScript-Bibliotheken von CDNs) nicht manipuliert wurden.

**Funktionsweise:**
1. Der Webentwickler berechnet einen kryptografischen Hash der erwarteten Ressource (z. B. SHA-384)
2. Dieser Hash wird als `integrity`-Attribut im HTML-Tag hinterlegt
3. Der Browser lädt die Ressource und vergleicht ihren Hash mit dem hinterlegten Wert
4. Bei Abweichung wird die Ressource blockiert und nicht ausgeführt

**Beispiel:**
```html
<script src="https://cdn.example.com/lib.js"
        integrity="sha384-abc123..."
        crossorigin="anonymous"></script>
```

**Schutz vor:**
- **Supply-Chain-Angriffe:** Manipulierte CDN-Inhalte werden erkannt und blockiert
- **Man-in-the-Middle-Angriffe:** Veränderte Ressourcen auf dem Transportweg werden abgewiesen

**Im IT-Grundschutz:** APP.1.2.A1 fordert, dass eingesetzte Webbrowser Maßnahmen zur Subresource Integrity unterstützen.
