---
aliases:
  - Open Relay
  - Mail-Relay
---
Ein **Spam-Relay** (auch *Open Relay*) bezeichnet einen E-Mail-Server, der E-Mails von beliebigen Absendern an beliebige Empfänger weiterleitet — also nicht auf eigene Nutzer oder Domains beschränkt ist.

**Problem:**
- Spam-Versender nutzen schlecht gesicherte Mail-Server als Relay, um Spam-E-Mails über fremde Infrastruktur zu versenden
- Die IP-Adresse des missbrauchten Servers erscheint als Absender, nicht die des eigentlichen Angreifers
- Folge: Der legitime Server-Betreiber wird auf Spam-Blacklisten gesetzt und verliert die Zustellbarkeit seiner eigenen E-Mails

**Schutzmaßnahmen:**
- **SMTP-Authentifizierung:** Nur authentifizierte Nutzer dürfen E-Mails über den Server versenden
- **Absender-Prüfung:** Weiterleitung nur für eigene Domains erlauben
- **IP-Allowlisting:** Nur bekannte Absender-IPs zulassen
- **SPF, DKIM, DMARC:** Protokolle zur Absender-Authentifizierung und -validierung

**Im IT-Grundschutz:** APP.5.3.A2 fordert, dass E-Mail-Server so konfiguriert werden, dass sie nicht als Spam-Relay missbraucht werden können.
