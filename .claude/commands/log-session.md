Analysiere die aktuelle Unterhaltung und trage die Ergebnisse in die heutige Daily Note ein. Gehe dabei Schritt für Schritt vor:

## Schritt 1 — Daily Note finden oder anlegen

Das heutige Datum ist $CURRENT_DATE. Das Format der Daily Notes ist `DD.MM.YYYY.md`, der Ordnerpfad ist:
`/home/jonas/Documents/obsidian_notes/Daily notes/MM.YYYY/DD.MM.YYYY.md`

Prüfe ob die Datei existiert. Falls nicht, erstelle sie mit folgendem Inhalt:
```
#dailynote

## Todays todos: 
- [ ] 

## Progress:

## Problems:

## Questions:

## New Notes:

## Noteworthy:

## Meetings:

## To Do's:
```

Lies danach die vorhandene Datei komplett.

## Schritt 2 — Neue Wiki-Notes erstellen

Schau dir alle Themen und Konzepte in der Unterhaltung an. Für jedes Thema das **neu** ist (kein existierender Wiki-Eintrag dazu vorhanden) und das fachlich relevant ist:

1. Erstelle eine neue Notiz unter dem passenden Pfad in `/home/jonas/Documents/obsidian_notes/Wiki 2.0/`
2. Die Notiz soll das Thema erklären, Quellenlinks enthalten (offizielle Docs, MDN, etc.), und Obsidian Wikilinks zu verwandten Themen setzen
3. Merke dir den Dateinamen für Schritt 3

Erstelle **keine** neue Note wenn zu dem Thema bereits eine existiert.

## Schritt 3 — Daily Note aktualisieren

Füge die Inhalte in die Daily Note ein — **immer anhängen, nie überschreiben** was schon drin steht:

### ## Progress:
Füge stichpunktartige Bullet Points hinzu was in dieser Session gemacht wurde. Konkret, kurz, in der Vergangenheitsform. Beispiele:
- Svelte Lehrplan erstellt (Phase 1–5, 16 Übungsaufgaben)
- Wiki-Einträge zu Svelte Komponenten und Reaktivität angelegt
- SAKE Projektstruktur analysiert

### ## New Notes:
Füge für jede in Schritt 2 erstellte Note einen Wikilink ein:
- [[Notizname]] — Ein-Satz-Beschreibung

### ## Problems:
Wenn in der Unterhaltung Verständnisfragen, Probleme oder offene Punkte aufgetaucht sind: fasse sie kurz zusammen und beantworte sie direkt darunter in 1–2 Sätzen. Format:
- **Problem/Frage:** Kurze Beschreibung
  → Kurze Antwort / Lösung

Wenn es keine Problems gab, diesen Abschnitt nicht ändern.

## Wichtige Regeln

- Bestehende Inhalte in der Daily Note **niemals löschen oder überschreiben**
- Immer am Ende des jeweiligen Abschnitts anhängen
- Keine leeren Abschnitte hinzufügen
- Datumspfad korrekt berechnen (Monat zweistellig, Jahr vierstellig)
- Wenn heute noch keine Daily Note existiert, erst anlegen, dann befüllen
- Zum Schluss: kurz berichten was eingetragen wurde
