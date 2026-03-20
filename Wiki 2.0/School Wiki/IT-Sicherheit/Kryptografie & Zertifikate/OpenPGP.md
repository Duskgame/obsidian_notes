---
aliases:
  - PGP
  - Pretty Good Privacy
  - GnuPG
  - GPG
---
[OpenPGP.org – Standard](https://www.openpgp.org/)

**OpenPGP** ist ein offener kryptografischer Standard (RFC 4880) für die Ende-zu-Ende-Verschlüsselung und digitale Signierung von E-Mails und Dateien.

**Grundprinzip (asymmetrische Kryptografie):**
- Jeder Nutzer hat ein **Schlüsselpaar**: einen öffentlichen Schlüssel (zum Verschlüsseln) und einen privaten Schlüssel (zum Entschlüsseln)
- Der öffentliche Schlüssel wird frei geteilt; der private Schlüssel bleibt geheim
- Nachrichten werden mit dem öffentlichen Schlüssel des Empfängers verschlüsselt und können nur mit dessen privatem Schlüssel entschlüsselt werden

**Vertrauensmodell:**
- OpenPGP verwendet ein dezentrales **Web of Trust** — Nutzer bestätigen gegenseitig die Echtheit ihrer öffentlichen Schlüssel
- Kein zentrales Zertifizierungssystem (im Gegensatz zu [[S/MIME]])

**Implementierungen:**
- **GnuPG (GPG):** Die verbreitetste Open-Source-Implementierung
- **Gpg4win:** Offizielle GnuPG-Distribution für Windows (beauftragt vom BSI)

**Abgrenzung zu [[S/MIME]]:**
- OpenPGP: Dezentrales Web of Trust, keine PKI erforderlich
- S/MIME: Hierarchische PKI mit Zertifizierungsstellen (CAs)

**Im IT-Grundschutz:** APP.1.1 und APP.5.3 empfehlen den Einsatz von Verschlüsselungs- und Signaturwerkzeugen wie Gpg4win (OpenPGP) für E-Mails und Dateien.
