![[Screenshot 2026-04-13 at 10-15-48 IT5ANS_LF5_Kriterienraster_LN1_und_LN2_2026.xlsx - LN2_LF05_2026_ERD.pdf.png|461]]


```mermaid
---
title: KrautUndRueben
---
erDiagram
	Kunde {
	int KundenNr
	string Vorname
	string Nachname
	string Geburtsdatum
	int AdressNr
	int KontaktNr
	}
	
	Kontaktinformationen {
	int KontaktNr
	string email
	int Telefon
	}
	
	Bestellung {
	int BestellNr
	int KundenNr
	int Rechnungsbetrag
	string Bestelldatum
	}
	
	Zutat {
	int ZutatNr
	int LieferantenNr
	string Bezeichnung
	int Bestand
	int Nettopreis
	} 
	 
	Naehrwert {
	string Bezeichnung
	string Einheit
	}
	
	Unvertraeglichkeit {
	int UnvertraeglichkeitsNr
	string Bezeichnung
	}
	
	
	
	Lieferant {
	int LieferantenNr
	string Lieferantenname
	int AdressNr
	int KontaktNr
	}
	
	Adresse {
	int AdressNr
	string Strasse
	int HausNr
	int PLZ
	}
	
	Rezepte {
	int RezeptNr
	string RezeptName
	}
	
	
	
```

---



```mermaid
---
title: KrautUndRueben
---
erDiagram
	Kunde {
		int KundenNr PK
		string Vorname
		string Nachname
		date Geburtsdatum
		string Email
		string Telefon
		int AdressID FK
	}

	Adresse {
		int AdressID PK
		string Strasse
		int HausNr
		string PLZ
		string Ort
	}

	Bestellung {
		int BestellNr PK
		int KundenNr FK
		date Bestelldatum
		float Gesamtpreis
	}

	Bestellung_mit_Rezept {
		int BestellNr FK
		int RezeptNr FK
		int Anzahl
	}

	Rezept {
		int RezeptNr PK
		string Name
	}

	Rezept_mit_Zutat {
		int RezeptNr FK
		int ZutatNr FK
		float Menge
	}

	Rezept_mit_Ernaehrungskategorie {
		int RezeptNr FK
		int ErnaehrungskategorieNr FK
	}

	Ernaehrungskategorie {
		int ErnaehrungskategorieNr PK
		string Bezeichnung
	}

	Zutat {
		int ZutatNr PK
		string Bezeichnung
		float Nettopreis
		int Menge
		string Mengeneinheit
	}

	Zutat_mit_Naehrwert {
		int ZutatNr FK
		int NaehrwertNr FK
		float Wert
	}

	Naehrwert {
		int NaehrwertNr PK
		string Bezeichnung
		string Einheit
	}

	Zutat_mit_Nachhaltigkeitsprofil {
		int ZutatNr FK
		int NachhaltigkeitsNr FK
		float Wert
	}

	Nachhaltigkeitsprofil {
		int NachhaltigkeitsNr PK
		string Bezeichnung
		string Einheit
	}

	Zutat_mit_Beschraenkung {
		int ZutatNr FK
		int BeschraenkungsNr FK
	}

	Beschraenkung {
		int BeschraenkungsNr PK
		string Bezeichnung
	}

	Lieferant {
		int LieferantID PK
		string Firmenname
		string Email
		string Telefon
		int AdressID FK
	}

	Zutat_von_Lieferant {
		int ZutatNr FK
		int LieferantID FK
	}

	Zertifizierung {
		int ZertifizierungNr PK
		string Bezeichnung
	}

	Lieferant_mit_Zertifizierung {
		int LieferantID FK
		int ZertifizierungNr FK
	}

	Kunde }o--|| Adresse : "wohnt in"
	Lieferant }o--|| Adresse : "ansaessig in"
	Kunde ||--o{ Bestellung : "erteilt"
	Bestellung ||--|{ Bestellung_mit_Rezept : "enthaelt"
	Rezept ||--|{ Bestellung_mit_Rezept : "bestellt in"
	Rezept ||--|{ Rezept_mit_Zutat : "besteht aus"
	Zutat ||--|{ Rezept_mit_Zutat : "verwendet in"
	Rezept ||--o{ Rezept_mit_Ernaehrungskategorie : "gehoert zu"
	Ernaehrungskategorie ||--o{ Rezept_mit_Ernaehrungskategorie : "kategorisiert"
	Zutat ||--o{ Zutat_mit_Naehrwert : "hat"
	Naehrwert ||--o{ Zutat_mit_Naehrwert : "beschreibt"
	Zutat ||--o{ Zutat_mit_Nachhaltigkeitsprofil : "hat"
	Nachhaltigkeitsprofil ||--o{ Zutat_mit_Nachhaltigkeitsprofil : "beschreibt"
	Zutat ||--o{ Zutat_mit_Beschraenkung : "hat"
	Beschraenkung ||--o{ Zutat_mit_Beschraenkung : "betrifft"
	Zutat ||--o{ Zutat_von_Lieferant : "geliefert von"
	Lieferant ||--o{ Zutat_von_Lieferant : "liefert"
	Lieferant ||--o{ Lieferant_mit_Zertifizierung : "hat"
	Zertifizierung ||--o{ Lieferant_mit_Zertifizierung : "gilt fuer"
```

---

ERD Konzept 2

Entität:

- Rezept
	- RezeptNr (PK)
	- Name

- Ernährungskategorie
	- ErnährungskategorieNr (PK)
	- Bezeichnung

- Kunde
	- KundenNr (PK)
	- AdressID (FK)
	- Vorname
	- Nachname
	- Geb. Dat.
	- Email
	- Telefon

- Bestellung
	- Bestellnr. (PK)
	- KundenNr (FK)
	- Bestelldatum
	- Gesamtpreis

- Lieferant
	- LieferantID (PK)
	- Firmenname
	- AdressID (FK)
	- Email
	- Telefon

- Zertifizierung 
	- ZertifizierungNr (PK)
	- Bezeichnung 

- Zutat
	- ZutatNr (PK)
	- Bezeichnung
	- Nettopreis
	- Bestand

- Naehrwert
	- NaehrwertNr (PK)
	- Bezeichnung 
	- Einheit 

- Nachhaltigkeitsprofil 
	- NachhaltigkeitsNr (PK)
	- Bezeichnung 
	- Einheit 

- Beschränkung
	- BeschraenkungsNr (PK)
	- Bezeichnung

- Einheit 
	- EinheitNr (PK)
	- Bezeichnung 

- Adresse
	- AdressID (PK)
	- Straße
	- HausNr
	- PLZ
	- Ort


Beziehung:

- Bestellung_mit_Rezept
	- BestellNr (FK)
	- RezeptNr (FK)
	- Anzahl

- Rezept_mit_Zutat
	- RezeptNr (FK)
	- ZutatNr (FK)
	- Menge
	- EinheitNr (FK) 

- Rezept_mit_Ernaehrungskategorie
	- RezeptNr (FK)
	- ErnährungskategorieNr (FK)

- Zutat_mit_Beschraenkung
	- ZutatNr (FK)
	- BeschraenkungsNr (FK)

- Zutat_mit_Naehrwert
	- ZutatNr (FK)
	- NaehrwertNr (FK)
	- Wert

- Zutat_mit_Nachhaltigkeitsprofil 
	- ZutatNr (FK)
	- NachhaltigkeitsNr (FK)
	- Wert

- Zutat_von_Lieferant
	- ZutatNr (FK)
	- LieferantID (FK)

- Lieferant_mit_Zertifizierung
	- LieferantID (FK)
	- ZertifizierungNr (FK)


1-1 beziehungen 
einheiten zu anderen entitäten