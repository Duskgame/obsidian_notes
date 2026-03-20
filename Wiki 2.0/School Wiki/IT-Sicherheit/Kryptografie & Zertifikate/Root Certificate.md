---
aliases:
  - Wurzelzertifikat
  - Root-Zertifikat
  - CA
  - Certificate Authority
  - Zertifizierungsstelle
  - Root CA
  - Stammzertifikat
---
[BSI – Public-Key-Infrastruktur (PKI)](https://www.bsi.bund.de/DE/Themen/Unternehmen-und-Organisationen/Informationen-und-Empfehlungen/Kryptographie/Kryptographie_node.html)

Ein **Root Certificate** (Wurzelzertifikat) ist das oberste Zertifikat in einer **Public-Key-Infrastruktur (PKI)**. Es wird von einer vertrauenswürdigen Zertifizierungsstelle (Root Certificate Authority, Root CA) selbstsigniert und bildet den Vertrauensanker für alle untergeordneten Zertifikate.

**Zertifikatskette (Chain of Trust):**
```
Root CA (selbstsigniert, im Browser hinterlegt)
└── Intermediate CA (von Root CA signiert)
    └── Server-Zertifikat (von Intermediate CA signiert)
```

**Vertrauensmodell:**
- Browser und Betriebssysteme liefern eine Liste vertrauenswürdiger Root CAs mit
- Ein Server-Zertifikat gilt als vertrauenswürdig, wenn seine Zertifikatskette zu einem dieser Root CAs zurückführt
- Gefälschte oder nicht autorisierte Root CAs ermöglichen [[Man-in-the-Middle|Man-in-the-Middle-Angriffe]]

**Verwaltung:**
- In Unternehmensumgebungen kann eine IT-Abteilung eigene Root CAs ausrollen (z. B. für interne Dienste oder TLS-Inspection)
- Benutzende sollten **keine eigenen Root-Zertifikate** hinzufügen können → Verwaltung per [[Group Policy Object (GPO)|GPO]]

**Im IT-Grundschutz:** APP.1.2.A3 fordert, dass nur der IT-Betrieb die Liste vertrauenswürdiger Wurzelzertifikate ändern darf.
