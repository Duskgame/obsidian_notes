---
aliases:
  - S/MIME
  - Secure/Multipurpose Internet Mail Extensions
  - X.509
  - S/MIME (X.509)
---
> **Quelle:** [RFC 5751 – Secure/Multipurpose Internet Mail Extensions (S/MIME)](https://www.rfc-editor.org/rfc/rfc5751)

**S/MIME** (Secure/Multipurpose Internet Mail Extensions) ist ein kryptografischer Standard für die Ende-zu-Ende-Verschlüsselung und digitale Signierung von E-Mails auf Basis von X.509-Zertifikaten. Der Standard ist in RFC 5751 spezifiziert.

**Funktionen:**
- **Verschlüsselung:** E-Mails werden mit dem öffentlichen Schlüssel des Empfängers verschlüsselt — nur der Empfänger kann sie entschlüsseln
- **Digitale Signatur:** Bestätigt die Herkunft und Unverändertheit der E-Mail

**Vertrauensmodell:**
- S/MIME basiert auf einer hierarchischen **Public-Key-Infrastruktur (PKI)**
- Zertifikate werden von vertrauenswürdigen Zertifizierungsstellen (Certificate Authorities, CAs) ausgestellt und signiert
- Breite Kompatibilität mit Unternehmens-E-Mail-Programmen (Outlook, Thunderbird, etc.)

**Abgrenzung zu [[OpenPGP]]:**
- S/MIME: Zentrale PKI mit CAs — Zertifikate müssen bezahlt oder intern ausgestellt werden
- OpenPGP: Dezentrales Web of Trust — keine PKI erforderlich

**Im IT-Grundschutz:** APP.1.1 empfiehlt den Einsatz von Verschlüsselungswerkzeugen wie Gpg4win, das sowohl [[OpenPGP]] als auch S/MIME (X.509) unterstützt.
