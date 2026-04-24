# Präsentation Sprechnotizen – Bewertungskriterien

**Fachgespräch IT-Grundschutz-Check Client C023 (MooveTeq GmbH)**

## Bewertungsübersicht

| Kr. | Inhalt | Gewicht | Max. Punkte | Wer |
|-----|--------|---------|-------------|-----|
| 2 | Mindmap vorstellen + Ursachen des Ausfalls begründet spekulieren | 3 | 30 | Jonas |
| 3 | Ist-Zustand des Clients + Aufstellungsort vorstellen | 2 | 20 | Person 2 |
| 4 | Bausteinauswahl aus INF, APP, SYS erläutern und begründen | 3 | 30 | Jonas |
| 5 | Aufbau des tabellarischen Prüfplans vorstellen | 2 | 20 | Person 2 |
| 6 | Ergebnisse der Schutzbedarfsanalyse anhand des Prüfplans vorstellen | 3 | 30 | Person 3 |
| 7 | Maßnahmen erläutern und begründen | 3 | 30 | Jonas |
| 10 | Visualisierung unterstützt Inhalte sinnvoll | 2 | 20 | alle |
| 11 | Art des Vortrags unterstützt Inhalte sinnvoll | 2 | 20 | alle |

---

## Vorgeschlagener Ablauf

```
Jonas        → Kriterium 2: Mindmap + Ursachen
Person 2     → Kriterium 3: Ist-Zustand Client + Aufstellungsort
Jonas        → Kriterium 4: Bausteinauswahl begründen
Person 2     → Kriterium 5: Aufbau Prüfplan erklären
Person 3     → Kriterium 6: Ergebnisse (INF.7 + SYS.2.1 + APP.5.3)
Jonas        → Kriterium 7: Maßnahmen
```

---

---

# KRITERIUM 2 – Mindmap + Ursachen des Ausfalls
**Wer: Jonas | Gewicht: 3**

### Sprechnotiz

„Bevor wir den IT-Grundschutz-Check durchgeführt haben, haben wir zunächst ein Brainstorming gemacht, um mögliche Schwachstellen der MooveTeq zu identifizieren. Das Ergebnis haben wir in dieser Mindmap festgehalten."

*[Mindmap zeigen – entweder Mermaid-Diagramm oder kits.app-Link]*

„Unsere Mindmap gliedert die Schwachstellen in vier Bereiche:"

**Software:**
- Windows als verbreitetes Ziel mit bekannten Standardschwachstellen
- Standard-Office und E-Mail-Clients als häufige Einfallstore
- Möglicherweise fehlende Detektion- und Reaktionssoftware

**Hardware:**
- Keine Backups in der Dokumentation erwähnt
- Keine Alarmanlagen in den Vertriebsstandorten → Diebstahlgefahr → physischer Zugang zu Geräten

**Mitarbeiter:**
- Abteilungen mit besonderem Schutzbedarf (Buchhaltung, Personal)
- Ungeklärte Zugriffsberechtigungen – ist Least Privilege (Prinzip der minimalen Rechtevergabe: jeder bekommt nur die Berechtigungen, die er wirklich braucht) umgesetzt?
- Phishing (gefälschte E-Mails, die Nutzer zur Preisgabe von Daten oder zum Ausführen von Schadsoftware verleiten) über die E-Mail-Clients an allen Arbeitsplätzen
- Malware (Schadsoftware) durch Spezialsoftware als Einfallstor

**Netzwerk:**
- Schnelsen ist für Internet abhängig von Ohlsdorf – Single Point of Failure (einzelne Schwachstelle, deren Ausfall das gesamte System lahmlegt)
- Standleitung als potenzieller Angriffspunkt für sensible Daten
- VPN-Nutzung (Virtual Private Network – verschlüsselter Tunnel für sichere Fernverbindungen ins Firmennetz) im Vertrieb als mögliche Schwachstelle

### Begründete Spekulation über den Ausfall

„Auf Basis dieser Analyse kommen für den Ausfall mehrere Szenarien in Frage:

Das wahrscheinlichste Szenario ist ein **Phishing-Angriff über E-Mail** – E-Mail-Clients sind an allen Arbeitsplätzen vorhanden, und es gibt keine dokumentierten Schulungen oder Awareness-Maßnahmen. Eine Schad-Mail mit aktivem Inhalt hätte bei ungepatchter Software leichtes Spiel.

Ein zweites Szenario wäre **Malware über installierte Drittanbieter-Software** – wir haben im Check tatsächlich Software wie Steam, Adobe Flash Player 11 und PunkBuster gefunden, die keinen beruflichen Einsatzzweck erkennbar haben und teils seit Jahren nicht mehr mit Sicherheitsupdates versorgt werden.

Denkbar wäre auch ein **Ausfall durch den Single Point of Failure im Netzwerk** – die Internetverbindung von Schnelsen ist abhängig von Ohlsdorf. Ein Ausfall dort würde den gesamten Standort treffen."

### Mögliche Lehrerfragen

**F: Warum haben Sie gerade diese vier Kategorien für das Brainstorming gewählt?**
A: Software, Hardware, Mitarbeiter und Netzwerk decken die klassischen Angriffsvektoren ab und entsprechen auch dem Aufbau des IT-Grundschutz-Kompendiums, das ebenfalls zwischen Anwendungen, Systemen, Infrastruktur und Prozessen unterscheidet.

**F: Welches Szenario halten Sie persönlich für das wahrscheinlichste?**
A: Phishing über E-Mail – weil es der niedrigschwelligste Angriff ist, keine technischen Kenntnisse erfordert und im Unternehmen keine Schutzmaßnahmen dagegen dokumentiert sind.

---

---

# KRITERIUM 3 – Ist-Zustand Client + Aufstellungsort
**Wer: Person 2 | Gewicht: 2**

### Sprechnotiz

„Ich stelle jetzt den Zustand des Clients C023 vor, den wir untersucht haben, sowie seinen Aufstellungsort."

### Der Client-PC

„Der Client C023 ist ein Windows-10-Pro-Rechner in der IT-Abteilung der MooveTeq. Er ist Mitglied einer Windows-Domäne (zentrales Unternehmensnetzwerk, das von einem Server administriert wird) und wird zentral administriert.

**Betriebssystem:** Windows 10 Pro – wichtig: nicht Enterprise  
**Installierte Software (Auswahl):**
- Microsoft Office Professional Plus 2013 (kein aktueller Herstellersupport mehr)
- Mozilla Thunderbird 68.6.0 (veraltet)
- Google Chrome 80 (veraltet)
- Microsoft Teams + Skype Meetings App
- Avira Antivirus (zentral gesteuert)
- Gpg4win (Software für E-Mail-Verschlüsselung nach OpenPGP-Standard, installiert aber ohne dokumentierte Nutzung)
- Adobe Flash Player 11 (seit 2020 abgekündigt, keine Updates mehr)
- Steam, PunkBuster Services, LEGO Digital Designer (kein beruflicher Zweck erkennbar)
- Dropbox, MozBackup

**UEFI / Firmware** *(Unified Extensible Firmware Interface – moderner Nachfolger des BIOS, steuert den PC-Startvorgang)*:
- Admin-Passwort gesetzt – allerdings ist dieses Passwort „QwErTz", also sehr schwach
- TPM-Chip (Trusted Platform Module – dedizierter Sicherheitschip auf dem Mainboard für Schlüsselspeicherung und Festplattenverschlüsselung) vorhanden, aber deaktiviert
- Kein Secure Boot (UEFI-Funktion, die nur vom Hersteller signierte Betriebssysteme beim Start zulässt)
- Boot-Reihenfolge: Festplatte, Netzwerk, USB, DVD, CD – USB-Boot also aktiv

**Weitere Auffälligkeiten:**
- PowerShell ExecutionPolicy (Sicherheitseinstellung, die steuert welche Skripte ausgeführt werden dürfen) auf `Unrestricted` (keinerlei Einschränkung – alle Skripte erlaubt) gesetzt
- Windows-Updates auf maximale Verzögerung konfiguriert
- Benutzergruppe hat Vollzugriff auf Laufwerk D:
- Kein Kensington-Schloss aktiv genutzt (vorhanden aber nicht eingesetzt)"

### Der Aufstellungsort

„Der Client steht im IT-Büro der MooveTeq. Das ist ein offener Büroraum mit ca. 40 m² für 5 IT-Mitarbeiter im Erdgeschoss.

Besonders auffällig:
- Die Teeküche nebenan wird von Mitarbeitern aller Abteilungen genutzt → regelmäßiger Publikumsverkehr im IT-Bereich
- Ein Besprechungsbereich im IT-Büro wird ebenfalls von allen Abteilungen genutzt → externe Personen im IT-Büro
- Im selben Raum steht ein Server – ohne gesonderte Absicherung
- Alle Büros außer der Geschäftsleitung sind für alle Mitarbeiter frei zugänglich
- Kein abschließbarer Stauraum – Unterlagen und Ersatzteile liegen auf offenem Regal und auf dem Boden
- Fliegende Verkabelung auf dem Boden"

### Mögliche Lehrerfragen

**F: Warum ist es relevant, welche Software installiert ist?**
A: Jede installierte Anwendung vergrößert die Angriffsfläche. Software, die nicht gebraucht wird, sollte nicht installiert sein. Veraltete Software ohne Sicherheitsupdates enthält bekannte, ausnutzbare Schwachstellen.

**F: Was bedeutet „Windows 10 Pro – nicht Enterprise" konkret für die Sicherheit?**
A: Windows 10 Enterprise erlaubt es, die Telemetrie vollständig zu deaktivieren (Level 0). Pro erlaubt das nicht – das ist eine BSI-Anforderung, die sich dann technisch nicht vollständig erfüllen lässt.

---

---

# KRITERIUM 4 – Bausteinauswahl begründen
**Wer: Jonas | Gewicht: 3**

### Sprechnotiz

„Nachdem wir den Ist-Zustand aufgenommen hatten, mussten wir entscheiden, welche BSI-Bausteine wir für die Prüfung heranziehen. Das BSI nennt diesen Schritt Modellierung des Informationsverbunds."

„Der Informationsverbund in unserem Fall besteht aus:
- dem Client-PC selbst
- der installierten Software
- seiner Einbettung ins Unternehmensnetzwerk
- und dem Aufstellungsort"

„Aus dem IT-Grundschutz-Kompendium haben wir folgende Bausteine ausgewählt:"

**APP – Anwendungen** *(für die installierte Software)*

| Baustein | Begründung |
|----------|-----------|
| APP.1.1 Office-Produkte | MS Office Professional Plus 2013 installiert |
| APP.1.2 Webbrowser | Google Chrome installiert |
| APP.5.3 E-Mail-Client und -Server | Mozilla Thunderbird installiert |
| APP.5.4 Unified Communications (UCC) | Teams + Skype Meetings App installiert |
| APP.6 Allgemeine Software | Gilt grundsätzlich für jede Software im Informationsverbund |

**SYS – IT-Systeme** *(für den Client-PC selbst)*

| Baustein | Begründung |
|----------|-----------|
| SYS.2.1 Allgemeiner Client | Gilt für alle Clients unabhängig vom Betriebssystem |
| SYS.2.2.3 Clients unter Windows | Ergänzend zu SYS.2.1 für Windows 10 spezifische Anforderungen |

*Wichtig: allgemeiner Baustein (SYS.2.1) wird immer zusammen mit dem spezifischen (SYS.2.2.3) verwendet.*

**INF – Infrastruktur** *(für den Aufstellungsort)*

| Baustein | Begründung |
|----------|-----------|
| INF.7 Büroarbeitsplatz | Client steht in einem Büroraum → dieser Baustein passt; INF.1 Allgemeines Gebäude wurde nicht modelliert, da nur ein Client betrachtet wird |

„Bausteine, die wir ausgeschlossen haben: z. B. SYS.1.1 Allgemeiner Server – wir betrachten keinen Server, sondern einen Client. APP.5.2 Microsoft Exchange – Thunderbird statt Outlook/Exchange."

### Mögliche Lehrerfragen

**F: Warum haben Sie INF.1 Allgemeines Gebäude nicht genommen?**
A: Das BSI gibt vor, dass INF.1 auf das gesamte Gebäude angewendet wird. Da wir nur einen einzelnen Client und seinen Aufstellungsort betrachten, reicht INF.7 Büroarbeitsplatz. INF.1 würde den Informationsverbund unnötig ausdehnen.

**F: Warum braucht man SYS.2.1 wenn es SYS.2.2.3 gibt?**
A: SYS.2.1 enthält allgemeine Anforderungen, die für alle Clients gelten – unabhängig vom Betriebssystem. SYS.2.2.3 ergänzt Windows-spezifische Anforderungen. Das BSI schreibt vor, immer den allgemeinen und den spezifischen Baustein gemeinsam anzuwenden.

---

---

# KRITERIUM 5 – Aufbau des tabellarischen Prüfplans
**Wer: Person 2 | Gewicht: 2**

### Sprechnotiz

„Ich erkläre jetzt, wie unser Prüfplan aufgebaut ist."

*[Prüfplan aufschlagen – z. B. die APP.5.3-Tabelle zeigen]*

„Für jeden ausgewählten Baustein haben wir eine Tabelle erstellt. Jede Zeile entspricht einer konkreten Anforderung aus dem BSI-Kompendium.

Die Tabelle hat drei Spalten:

**Spalte 1 – ID und Anforderung:**
Die BSI-ID (z. B. APP.5.3.A1) und der genaue Wortlaut der Anforderung. Das BSI unterscheidet dabei zwischen MUSS (Basisanforderung, zwingend) und SOLLTE (Standard, empfohlen).

**Spalte 2 – Status:**
Hier haben wir eingetragen, ob die Anforderung erfüllt ist: *ja / nein / teilweise*. Das ist das eigentliche Prüfergebnis.

**Spalte 3 – Umsetzung:**
Hier haben wir begründet, warum wir den jeweiligen Status vergeben haben – also welche Informationen aus den MooveTeq-Dokumenten zu dieser Einschätzung geführt haben.

Zum Beispiel: *APP.5.3.A1 verlangt, dass E-Mail-Clients HTML-Inhalte nicht automatisch interpretieren. Status: nein – weil in keiner der Quelldokumente eine entsprechende Thunderbird-Konfiguration beschrieben ist.*

So entsteht am Ende für jeden Baustein eine vollständige Übersicht, welche Anforderungen erfüllt, nicht erfüllt oder nur teilweise erfüllt sind."

### Mögliche Lehrerfragen

**F: Woher kommen die Anforderungen in der Tabelle?**
A: Direkt aus dem BSI IT-Grundschutz-Kompendium. Das BSI stellt für jeden Baustein auch Checklisten zum Download bereit, die wir als Grundlage genutzt haben.

**F: Was ist der Unterschied zwischen MUSS und SOLLTE?**
A: MUSS-Anforderungen sind Basisanforderungen – sie müssen zwingend erfüllt werden. SOLLTE-Anforderungen sind Standard und sollten im Regelfall umgesetzt werden, können aber in begründeten Ausnahmen abgewichen werden. Es gibt noch KANN-Anforderungen für erhöhten Schutzbedarf, die wir auf Basisniveau nicht betrachtet haben.

---

---

# KRITERIUM 6 – Ergebnisse der Schutzbedarfsanalyse
**Wer: Person 3 | Gewicht: 3**

*Drei Bausteine exemplarisch vorstellen: INF.7, SYS.2.1, APP.5.3*

### Sprechnotiz Einstieg

„Ich stelle jetzt die Ergebnisse unserer Schutzbedarfsanalyse vor – exemplarisch an drei Bausteinen, je einen aus INF, SYS und APP."

---

### INF.7 – Büroarbeitsplatz

„Beim Büroarbeitsplatz haben wir geprüft, ob der Raum für einen sicherheitsrelevanten IT-Arbeitsplatz geeignet ist.

**Ergebnis:** Beide geprüften Anforderungen (A1 und A2) sind *nicht erfüllt*.

Die kritischsten Punkte aus dem Prüfplan:
- A1: Der Raum wird von Mitarbeitern aller Abteilungen betreten (Besprechungsbereich, Teeküche nebenan) → Publikumsverkehr in einem sicherheitsrelevanten Bereich, in dem außerdem ein Server steht
- A1: Kein abschließbarer Stauraum für vertrauliche Unterlagen oder Ersatzteile
- A2: Keine dokumentierte Regelung, dass Fenster beim Verlassen geschlossen und Türen abgesperrt werden"

---

### SYS.2.1 – Allgemeiner Client

„Beim allgemeinen Client haben wir fünf Anforderungen geprüft.

**Ergebnis:** Vier davon nicht erfüllt, eine teilweise.

Die wichtigsten Befunde:
- A1 (Authentisierung): Keine Bildschirmsperre konfiguriert – Status *nein*
- A3 (Autoupdate): OS-Updates auf maximale Verzögerung gesetzt, kein zentrales Patch-Management (systematische Verwaltung und Verteilung von Software-Updates) – Status *nein*
- A6 (Schadsoftware): Avira zentral vorhanden, aber regelmäßige Vollscans nicht dokumentiert, kein Incident-Response-Prozess (festgelegter Ablauf für den Umgang mit Sicherheitsvorfällen) – Status *teilweise*
- A8 (Bootvorgang): UEFI-Passwort „QwErTz" sehr schwach, TPM deaktiviert, USB-Boot aktiv, kein Secure Boot – Status *nein*
- A42 (Cloud-Funktionen): Telemetrie (automatische Übermittlung von Nutzungsdaten an Microsoft) auf Windows Standard statt minimiert, Dropbox installiert – Status *teilweise / nein*"

---

### APP.5.3 – Allgemeiner E-Mail-Client und -Server

„Beim E-Mail-Baustein haben wir die meisten Anforderungen geprüft – vier Unteranforderungen mit insgesamt etwa 20 Einzelpunkten.

**Ergebnis:** Überwiegend *nein*, einige *teilweise*.

Die wichtigsten Befunde:
- A1 (Sichere Konfiguration): HTML und aktive Inhalte in Thunderbird nicht deaktiviert, keine Transportverschlüsselung (Verschlüsselung der Daten auf dem Übertragungsweg) konfiguriert – Status *nein*
- A2 (Serverbetrieb): Keine Dokumentation zur serverseitigen Konfiguration vorhanden – weder TLS (Transport Layer Security – Verschlüsselungsprotokoll für Netzwerkübertragungen), noch DoS-Schutz (Schutz vor Denial-of-Service-Angriffen – Überlastungsangriffe, die einen Dienst zum Absturz bringen), noch Relay-Schutz (verhindert, dass der E-Mail-Server als Verteiler für fremde Spam-Mails missbraucht wird) – Status *nein*
- A3 (Datensicherung): Thunderbird speichert E-Mails lokal per POP3 (Post Office Protocol – lädt E-Mails auf den Client herunter, danach sind sie nur noch lokal vorhanden), MozBackup installiert aber ohne geregelten Einsatz – Status *teilweise / nein*
- A4 (Spam/Virus): Clientseitig Avira vorhanden, aber serverseitige Spam-Prüfung nicht dokumentiert, keine Abstimmung mit Datenschutzbeauftragten – Status *teilweise / nein*"

### Mögliche Lehrerfragen

**F: Warum ist „teilweise" kein gutes Ergebnis?**
A: Eine Anforderung die nur teilweise erfüllt ist, bietet nur teilweisen Schutz. Eine Kette ist so stark wie ihr schwächstes Glied – wenn die Dokumentation fehlt oder die Umsetzung unklar ist, kann im Ernstfall niemand belegen, dass die Anforderung wirklich eingehalten wird.

**F: Was bedeutet es, dass die Serverkonfiguration nicht dokumentiert ist?**
A: Das BSI betrachtet nicht nur den technischen Zustand, sondern auch die Nachvollziehbarkeit. Wenn eine Sicherheitsmaßnahme nicht dokumentiert ist, gilt sie für den Grundschutz als nicht nachgewiesen – selbst wenn sie technisch umgesetzt wäre.

---

---

# KRITERIUM 7 – Maßnahmen erläutern und begründen
**Wer: Jonas | Gewicht: 3**

### Sprechnotiz

„Für alle nicht erfüllten Anforderungen haben wir Maßnahmen entwickelt und nach Priorität sortiert. Ich stelle die wichtigsten vor."

### Sofortmaßnahmen

**Adobe Flash Player 11 deinstallieren** *(APP.6)*
„Flash Player ist seit 2020 abgekündigt und erhält keine Sicherheitsupdates mehr. Er enthält bekannte, aktiv ausgenutzte Schwachstellen. Deinstallation ist sofort möglich und ohne Aufwand."

**UEFI-Passwort ersetzen + TPM aktivieren + BitLocker einrichten** *(SYS.2.1)*
„Das aktuelle UEFI-Passwort ‚QwErTz' schützt nicht. Ein starkes Passwort muss her. Gleichzeitig: TPM aktivieren und BitLocker (Windows-Festplattenverschlüsselung) einrichten, damit die Festplatte verschlüsselt wird. Der Recovery Key (Wiederherstellungsschlüssel für den Notfall) kommt ins Active Directory (Microsoft-Verzeichnisdienst für zentrale Nutzerverwaltung im Unternehmensnetzwerk). Ohne das ist die Festplatte bei Diebstahl ungeschützt."

**Server aus dem IT-Büro in gesicherten Bereich verlagern** *(INF.7)*
„Ein Server im offenen Büroraum mit Publikumsverkehr ist physisch nicht gesichert. Physischer Zugriff umgeht alle Software-Schutzmaßnahmen."

### Kurzfristige Maßnahmen

**Bildschirmsperre per GPO erzwingen** *(SYS.2.1)*
„Nach spätestens 10 Minuten Inaktivität automatisch sperren, Entsperrung nur per Passwort. Per GPO (Group Policy Object – zentrale Konfigurationsrichtlinie für alle Windows-PCs in der Domäne) ausrollbar, Benutzer können es nicht deaktivieren."

**Thunderbird härten** *(APP.5.3)*
„HTML-Darstellung deaktivieren, TLS (Transport Layer Security – Verschlüsselungsprotokoll) für alle Konten erzwingen, externe Inhalte blockieren – per AutoConfig-Datei (Thunderbird-Mechanismus für zentrale Konfigurationsverteilung im Netzwerk) zentral auf alle Instanzen ausrollen."

**USB-Boot deaktivieren, Secure Boot aktivieren** *(SYS.2.1)*
„Boot-Reihenfolge auf Festplatte beschränken. Verhindert, dass jemand ein fremdes Betriebssystem von USB startet und damit alle Festplattendaten liest."

### Mittelfristige Maßnahmen (Auswahl)

**IMAP-Umstellung oder geregeltes E-Mail-Backup** *(APP.5.3)*
„Thunderbird speichert lokal per POP3 – bei Festplattenausfall sind alle E-Mails verloren. Lösung: Auf IMAP (Internet Message Access Protocol – E-Mails verbleiben auf dem Server, der Client zeigt sie nur an) umstellen, dann liegen E-Mails auf dem Server und werden zentral gesichert."

**Zugangskonzept für IT-Büro** *(INF.7)*
„Technische oder organisatorische Maßnahme (z. B. Türcode), die den Zugang zum IT-Bereich auf befugte Personen beschränkt."

**Benutzerschulungen** *(APP.1.1, APP.5.3, APP.5.4)*
„Schulungen zu Office-Makros, Phishing-E-Mails und sicherer UCC-Nutzung (Unified Communications and Collaboration – kombinierte Plattformen für Kommunikation und Zusammenarbeit, z. B. Teams). Langfristig, weil Planung und Durchführung Zeit erfordern – aber wirkungsvoller als viele technische Maßnahmen."

### Priorisierungsbegründung

„Wir haben nach zwei Kriterien priorisiert: Schadenpotenzial und Umsetzungsaufwand. Sofortmaßnahmen haben hohes Schadenpotenzial bei geringem Aufwand. Langfristige Maßnahmen wie Schulungen oder der Windows-Enterprise-Wechsel sind wichtig, aber kosten- und zeitintensiv."

### Mögliche Lehrerfragen

**F: Warum haben Sie Benutzerschulungen als langfristig eingestuft, obwohl Phishing als wahrscheinlichste Ursache gilt?**
A: Die Einstufung bezieht sich auf den Umsetzungsaufwand, nicht auf die Wichtigkeit. Eine Schulung muss geplant, mit Datenschutzbeauftragten und Personalvertretung abgestimmt und durchgeführt werden. Technische Maßnahmen wie die Bildschirmsperre per GPO lassen sich dagegen in Minuten ausrollen.

**F: Welche Maßnahme hätte den größten Effekt mit dem geringsten Aufwand?**
A: Adobe Flash Player deinstallieren – das ist ein bekannter, aktiv ausgenutzter Angriffsvektor, die Deinstallation dauert Minuten und hat keinerlei Nachteile, da Flash produktiv nicht mehr genutzt wird.

**F: Wer ist für die Umsetzung der Maßnahmen zuständig?**
A: Der IT-Betrieb – also die IT-Abteilung der MooveTeq. Einige Maßnahmen (z. B. E-Mail-Server-Konfiguration, Abstimmung mit Datenschutzbeauftragten) erfordern zusätzlich die Einbeziehung weiterer Stellen.
