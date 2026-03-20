---
aliases:
  - CRL
  - Certificate Revocation List
  - Zertifikatssperrliste
---
[RFC 5280 – Internet X.509 PKI Certificate and CRL Profile](https://www.rfc-editor.org/rfc/rfc5280)

**CRL (Certificate Revocation List)** ist eine von einer Zertifizierungsstelle (CA) veröffentlichte Liste widerrufener Zertifikate. Webbrowser und andere TLS-Clients können anhand dieser Liste prüfen, ob ein Zertifikat noch gültig ist.

**Aufbau:**
- Enthält Seriennummern und Widerrufszeitpunkte aller widerrufenen Zertifikate
- Wird von der CA digital signiert und regelmäßig aktualisiert
- Abrufbar über einen in Zertifikaten eingebetteten CRL Distribution Point (CDP)

**Ablauf:**
1. TLS-Verbindung aufgebaut, Zertifikat erhalten
2. Browser lädt die CRL von der angegebenen URL herunter
3. Browser prüft, ob die Seriennummer des Zertifikats in der CRL enthalten ist
4. Bei Treffer: Verbindung wird abgelehnt

**Nachteile gegenüber [[OCSP (Online Certificate Status Protocol)|OCSP]]:**
- CRL-Dateien können sehr groß werden (viele hundert KB)
- Nur so aktuell wie das letzte Update-Intervall (z. B. 24h)
- Download-Overhead bei jeder Verbindungsprüfung

**Moderne Alternative:** [[CRLSets]] (Google Chrome) aggregiert CRL-Daten effizient in einer lokalen Datei.

**Im IT-Grundschutz:** APP.1.2.A2 fordert, dass Webbrowser widerrufene Zertifikate erkennen und ablehnen.
