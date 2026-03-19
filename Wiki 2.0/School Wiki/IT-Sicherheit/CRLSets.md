---
aliases:
  - CRL Sets
  - Chrome CRLSets
---
> **Quelle:** [Chromium – CRLSets](https://www.chromium.org/Home/chromium-security/crlsets/)

**CRLSets** ist ein von Google Chrome verwendeter Mechanismus zum schnellen Widerruf von TLS-Zertifikaten, der als Alternative zu den langsameren traditionellen Methoden [[OCSP (Online Certificate Status Protocol)|OCSP]] und [[CRL (Certificate Revocation List)|CRL]] entwickelt wurde.

**Problem mit klassischen Widerrufsmechanismen:**
- [[OCSP (Online Certificate Status Protocol)|OCSP]]-Abfragen sind langsam und beeinträchtigen den Seitenaufbau
- Bei OCSP-Ausfällen verhalten sich Browser oft nachsichtig (Soft-Fail) und akzeptieren Zertifikate trotzdem
- [[CRL (Certificate Revocation List)|CRL]]-Dateien können sehr groß werden

**Funktionsweise CRLSets:**
- Google sammelt widerrufene Zertifikate aus mehreren CRL-Quellen
- Diese werden zu einer kompakten, signierten Datei zusammengefasst
- Chrome lädt diese Datei regelmäßig im Hintergrund herunter (kein Warten beim Seitenaufruf)
- Bei TLS-Verbindungen prüft Chrome lokal, ob das Zertifikat widerrufen wurde

**Einschränkungen:**
- Enthält nur kritische Widerrufsfälle (z. B. kompromittierte CAs), nicht alle widerrufenen Zertifikate
- Kein vollständiger Ersatz für OCSP — ergänzendes Verfahren

**Im IT-Grundschutz:** APP.1.2.A2 fordert, dass Webbrowser widerrufene Zertifikate erkennen und ablehnen.
