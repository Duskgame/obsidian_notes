---
aliases:
  - Incident-Response-Prozess
  - Sicherheitsvorfallbehandlung
  - IR
---
[NIST SP 800-61 Rev. 2 – Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Incident Response** bezeichnet den strukturierten Prozess zur Erkennung, Analyse, Eindämmung und Behebung von IT-Sicherheitsvorfällen (Incidents).

**Typische Phasen nach [NIST SP 800-61](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final):**
1. **Vorbereitung:** Richtlinien, Tools und Verantwortlichkeiten festlegen
2. **Erkennung & Analyse:** Vorfall identifizieren, Ausmaß bewerten
3. **Eindämmung:** Betroffene Systeme isolieren (z. B. Netzwerktrennung)
4. **Beseitigung:** Schadsoftware entfernen, Schwachstelle schließen
5. **Wiederherstellung:** Systeme aus Backup wiederherstellen, Betrieb aufnehmen
6. **Nachbereitung:** Ursache dokumentieren, Lessons Learned

**Relevanz für C023:**
- Das BSI fordert (SYS.2.1.A6): Bei Schadsoftware-Infektion MUSS im Offline-Betrieb untersucht werden, ob Daten ausgespäht oder Schutzfunktionen deaktiviert wurden
- Ohne dokumentierten Incident-Response-Prozess ist eine geordnete Reaktion auf Vorfälle nicht möglich

**Im IT-Grundschutz:** SYS.2.1.A6 sowie allgemein der Baustein DER.2.1 (Behandlung von Sicherheitsvorfällen).
