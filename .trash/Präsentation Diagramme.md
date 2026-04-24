# Präsentation Diagramme – IT-Grundschutz-Check C023

---

## Folie 1 – Mindmap Schwachstellen (Kr. 2)

```mermaid
mindmap
	)Mögliche Schwachstellen bzw. Angriffspunkte
	bei der MooveTeq(
		((Software))
			Detektion und Reaktionssoftware?
			Windows Standardschwachstelle
				Standard-Office + E-Mail-Clients
				Zentral administriertes Windows-System
		((Hardware))
			Keine Backups erwähnt
			Keine Alarmanlagen in Vertriebsstandorten
				Gefahr durch Diebstahl und damit Zugang
		((Mitarbeiter))
			Abteilungen mit besonderem Schutzbedarf
				Buchhaltung
				Personal
			Zugriffsberechtigungen in hierarchischen Unternehmen
				Zugangsberechtigung zu Servern
				Least Privilege?
			Menschliche Faktoren
				Phishing
					E-Mail-Clients an allen Arbeitsplätzen
				Malware
					Spezialsoftware als Angriffspunkt
		((Netzwerk und Internet))
			Schnelsen für Internet abhängig von Ohlsdorf
			Standleitung als Angriffspunkt
			Vertrieb VPN-Nutzung
```

---

## Folie 2 – Bausteinauswahl (Kr. 4)

```mermaid
flowchart TB
    VB["🖥️ Informationsverbund\nClient C023 – MooveTeq"]

    VB --> INF
    VB --> SYS
    VB --> APP

    subgraph INF["INF – Infrastruktur"]
        INF7["INF.7\nBüroarbeitsplatz"]
    end

    subgraph SYS["SYS – IT-Systeme"]
        SYS21["SYS.2.1\nAllgemeiner Client"]
        SYS223["SYS.2.2.3\nWindows Client"]
        SYS21 -- "immer gemeinsam" --> SYS223
    end

    subgraph APP["APP – Anwendungen"]
        APP11["APP.1.1\nOffice-Produkte"]
        APP12["APP.1.2\nWebbrowser"]
        APP53["APP.5.3\nE-Mail-Client"]
        APP54["APP.5.4\nUCC – Teams/Skype"]
        APP6["APP.6\nAllgemeine Software"]
    end
```

---

## Folie 3 – Aufbau Prüfplan (Kr. 5)

**Beispiel: APP.5.3 – Allgemeiner E-Mail-Client**

| ID | Anforderung (MUSS/SOLLTE) | Status | Umsetzung / Begründung |
|----|--------------------------|--------|------------------------|
| APP.5.3.A1 | E-Mail-Clients MÜSSEN so konfiguriert werden, dass HTML-Code und aktive Inhalte nicht automatisch interpretiert werden. | ❌ nein | Keine spezifische Thunderbird-Konfiguration dokumentiert. |
| APP.5.3.A1 | E-Mail-Clients MÜSSEN für die Kommunikation eine sichere Transportverschlüsselung einsetzen. | ❌ nein | Keine TLS-Konfiguration für Thunderbird-Konten dokumentiert. |
| APP.5.3.A3 | Der IT-Betrieb MUSS die Daten der E-Mail-Clients regelmäßig sichern. | ⚠️ teilweise | Allgemeine Datensicherung vorhanden, aber keine spezifische Regelung für lokale Thunderbird-Daten (POP3). |

---

## Folie 4 – Ergebnisse Schutzbedarfsanalyse (Kr. 6)

**INF.7 – Büroarbeitsplatz**

| Anforderung | Status | Kritischer Befund |
|-------------|--------|-------------------|
| A1: Geeigneter Büroraum | ❌ nein | Besprechungsbereich + Teeküche → Publikumsverkehr im IT-Bereich; Server ungesichert im Raum |
| A2: Fenster und Türen | ❌ nein | Keine Regelung zum Schließen/Abschließen dokumentiert |

**SYS.2.1 – Allgemeiner Client**

| Anforderung             | Status    | Kritischer Befund                                                                   |
| ----------------------- | --------- | ----------------------------------------------------------------------------------- |
| A1: Bildschirmsperre    | ❌ nein    | Keine Bildschirmsperre konfiguriert                                                 |
| A3: OS-Updates          | ❌ nein    | Updates auf maximale Verzögerung gesetzt, kein WSUS                                 |
| A6: Schadsoftwareschutz | teilweise | Avira zentral vorhanden, Vollscans und Incident Response nicht dokumentiert         |
| A8: Bootvorgang / UEFI  | teilweise | UEFI-Passwort „QwErTz" (schwach); TPM deaktiviert; USB-Boot aktiv; kein Secure Boot |
| A42: Cloud-Funktionen   | teilweise | Telemetrie auf Windows Standard; Dropbox installiert                                |

**APP.5.3 – Allgemeiner E-Mail-Client**

| Anforderung               | Status    | Kritischer Befund                                                         |
| ------------------------- | --------- | ------------------------------------------------------------------------- |
| A1: Sichere Konfiguration | teilweise | HTML-Inhalte nicht deaktiviert; keine TLS-Konfiguration dokumentiert      |
| A2: Serverbetrieb         | ❌ nein    | Keine Dokumentation zu TLS, DoS-Schutz, Relay-Schutz                      |
| A3: Datensicherung        | teilweise | POP3 → E-Mails lokal; MozBackup installiert, aber kein geregelter Einsatz |
| A4: Spam-/Virenschutz     | teilweise | Clientseitig Avira vorhanden; serverseitige Prüfung nicht dokumentiert    |

---

## Folie 5 – Maßnahmen nach Priorität (Kr. 7)

```mermaid
quadrantChart
    title Massnahmen – Schadenpotenzial vs. Umsetzungsaufwand
    x-axis Geringer Aufwand --> Hoher Aufwand
    y-axis Geringes Schadenpotenzial --> Hohes Schadenpotenzial
    quadrant-1 Sofort umsetzen
    quadrant-2 Strategisch planen
    quadrant-3 Niedrige Prioritaet
    quadrant-4 Mittelfristig planen
    Flash deinstallieren: [0.05, 0.72]
    UEFI-Passwort ersetzen: [0.10, 0.76]
    PowerShell einschraenken: [0.08, 0.64]
    Kensington-Schloss nutzen: [0.06, 0.42]
    TPM + BitLocker einrichten: [0.22, 0.92]
    Bildschirmsperre per GPO: [0.15, 0.62]
    Chrome + Thunderbird updaten: [0.13, 0.58]
    USB-Boot deaktivieren: [0.12, 0.66]
    Thunderbird haerten: [0.20, 0.78]
    Server verlagern: [0.55, 0.88]
    IMAP-Umstellung: [0.48, 0.52]
    Zugangskontrolle IT-Buero: [0.60, 0.70]
    Benutzerschulungen: [0.82, 0.88]
    Office 2013 abloesen: [0.85, 0.68]
```
