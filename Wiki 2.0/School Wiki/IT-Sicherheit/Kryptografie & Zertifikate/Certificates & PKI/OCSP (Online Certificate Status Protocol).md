---
aliases:
  - OCSP
  - Online Certificate Status Protocol
  - OCSP Stapling
---
[RFC 6960 – Online Certificate Status Protocol (OCSP)](https://www.rfc-editor.org/rfc/rfc6960)

**OCSP (Online Certificate Status Protocol)** ist ein Internetprotokoll zur Echtzeit-Prüfung, ob ein TLS-Zertifikat widerrufen wurde. Es ist die modernere Alternative zur [[CRL (Certificate Revocation List)|Certificate Revocation List (CRL)]].

**Funktionsweise:**
1. Browser baut TLS-Verbindung auf, erhält Server-Zertifikat
2. Browser sendet OCSP-Anfrage an den OCSP-Responder der Zertifizierungsstelle
3. Responder antwortet: `good`, `revoked` oder `unknown`
4. Bei `revoked` bricht der Browser die Verbindung ab

**OCSP Stapling:**
- Problem: OCSP-Abfragen verlangsamen den Verbindungsaufbau und verraten dem CA-Betreiber, welche Seiten besucht werden
- Lösung: Der Server fragt selbst regelmäßig beim OCSP-Responder an und "heftet" (*stapled*) die signierte Antwort an das TLS-Handshake
- Vorteil: Kein zusätzlicher Roundtrip für den Browser, bessere Privatsphäre

**Soft-Fail vs. Hard-Fail:**
- Browser reagieren bei OCSP-Ausfällen meist mit **Soft-Fail** (Verbindung trotzdem erlaubt)
- Dies ermöglicht theoretisch Angriffe durch OCSP-Blockierung

**Im IT-Grundschutz:** APP.1.2.A2 fordert, dass Webbrowser widerrufene Zertifikate erkennen und ablehnen (z. B. via OCSP oder [[CRLSets]]).
