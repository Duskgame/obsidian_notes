---
aliases:
  - digitale Signaturen
  - Digitale Signierung
  - Code Signing
  - kryptografische Signatur
---
> **Quelle:** [BSI – Kryptographie und Schlüsselmanagement](https://www.bsi.bund.de/DE/Themen/Unternehmen-und-Organisationen/Informationen-und-Empfehlungen/Kryptographie/Kryptographie_node.html)

Eine **digitale Signatur** ist ein kryptografisches Verfahren, das die **Authentizität** (Herkunft) und **Integrität** (Unverändertheit) von Daten, Dokumenten oder Software sicherstellt.

**Funktionsweise:**
1. Der Ersteller berechnet einen **Hash-Wert** der Daten
2. Dieser Hash wird mit dem **privaten Schlüssel** des Erstellers verschlüsselt → das ist die Signatur
3. Der Empfänger entschlüsselt die Signatur mit dem **öffentlichen Schlüssel** des Erstellers
4. Der Empfänger berechnet selbst den Hash der empfangenen Daten
5. Stimmen beide Hashes überein, sind Authentizität und Integrität bestätigt

**Anwendungsfälle:**
- **Software-Signierung (Code Signing):** Hersteller signieren Installationspakete, um die Herkunft zu garantieren
- **E-Mail-Signierung:** Mit [[OpenPGP]] oder [[S/MIME]] signierte E-Mails belegen Absenderidentität
- **Dokumentensignaturen:** Rechtlich verbindliche Unterzeichnung digitaler Dokumente

**Sicherheitsgarantien:**
- **Authentizität:** Nachweis, wer die Daten erstellt/signiert hat
- **Integrität:** Nachweis, dass die Daten nicht verändert wurden
- **Nichtabstreitbarkeit:** Der Ersteller kann die Signatur nicht leugnen

**Im IT-Grundschutz:** APP.6.A4 fordert, dass digitale Signaturen von Installationspaketen zur Integritätsprüfung herangezogen werden, sofern verfügbar.
