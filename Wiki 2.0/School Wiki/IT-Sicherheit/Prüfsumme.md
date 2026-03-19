---
aliases:
  - Prüfsummen
  - Checksum
  - Hash
  - Hash-Wert
  - MD5
  - SHA-256
---
Eine **Prüfsumme** (auch *Checksum* oder *Hash-Wert*) ist ein numerischer Wert, der aus den Daten einer Datei durch eine mathematische Funktion (Hash-Funktion) berechnet wird und als kompakter Fingerabdruck dieser Datei dient.

**Verwendungszweck:**
- **Integritätsprüfung:** Prüfsummen ermöglichen den Nachweis, ob eine Datei verändert wurde
- **Übertragungsprüfung:** Erkennung von Übertragungsfehlern bei Downloads
- **Software-Verifikation:** Hersteller veröffentlichen Prüfsummen ihrer Downloads, damit Nutzer die Echtheit prüfen können

**Gängige Hash-Algorithmen:**
| Algorithmus | Hash-Länge | Sicherheit |
|-------------|-----------|------------|
| MD5 | 128 Bit | unsicher (Kollisionen möglich), nur zur Fehlererkennung |
| SHA-1 | 160 Bit | veraltet |
| SHA-256 | 256 Bit | sicher, empfohlen |
| SHA-512 | 512 Bit | sehr sicher |

**Abgrenzung zur [[Digitale Signatur|digitalen Signatur]]:**
- Eine Prüfsumme belegt nur die Integrität, nicht die Authentizität (Herkunft)
- Eine digitale Signatur belegt beides — wer die Datei erstellt hat und dass sie unverändert ist

**Im IT-Grundschutz:** APP.6.A4 fordert, dass Prüfsummen von Installationspaketen zur Integritätsprüfung herangezogen werden, sofern verfügbar.
