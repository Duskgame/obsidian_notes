# Svelte Lehrplan — SAKE Frontend

**Ziel:** Svelte 5 + SvelteKit 2 + TypeScript + Tailwind so weit verstehen, dass du das SAKE-Frontend lesen, verstehen und erweitern kannst.

**Gesamtdauer:** ~5 Wochen, ~1–1,5h pro Tag
**Ressourcen:** [learn.svelte.dev](https://learn.svelte.dev) (interaktives Tutorial), [[Svelte]] (Wiki-Index)

---

## Phase 1 — Svelte Grundlagen
*Ziel: Eine eigene Svelte-Komponente von Grund auf schreiben können.*
*Dauer: ~1,5 Wochen*

### 1.1 Theorie lesen
- [x] [[Svelte Komponenten]] — `.svelte`-Dateistruktur, Ausdrücke, Imports, Bindings
- [ ] [[Svelte Reaktivität (Runes)]] — `$state`, `$derived`, `$effect`
- [ ] [[Svelte Template-Logik]] — `{#if}`, `{#each}`, `{#await}`
- [ ] [[Svelte Props und Events]] — `$props()`, Event-Handler

### 1.2 Interaktives Tutorial durcharbeiten
- [ ] [Svelte Grundlagen-Tutorial](https://learn.svelte.dev/tutorial/welcome-to-svelte) (Kapitel 1–4)
  - Kapitel 1: Introduction
  - Kapitel 2: Reactivity
  - Kapitel 3: Props
  - Kapitel 4: Logic

### 1.3 Übungsaufgaben
- [ ] **Übung A — Zähler:** Erstelle eine Komponente mit einem Button, der einen Zähler hochzählt. Zeige den doppelten Wert daneben mit `$derived` an.
- [ ] **Übung B — Filterliste:** Erstelle eine Liste von 5 fiktiven Service-Account-Keys. Füge ein Textfeld hinzu, das die Liste nach dem Key-Namen filtert. Verwende `$derived` für die gefilterte Liste.
- [ ] **Übung C — Formular:** Erstelle ein Formular mit zwei Pflichtfeldern (JIRA-Ticket, Service Account E-Mail). Zeige einen Submit-Button nur an, wenn beide Felder ausgefüllt sind (`{#if}`).
- [ ] **Übung D — Komponente aufteilen:** Extrahiere den Key-Eintrag aus Übung B in eine eigene `KeyRow.svelte`-Komponente. Übergib die Daten als Props.

### 1.4 Checkpoint
- [ ] Ich kann eine `.svelte`-Datei aus dem SAKE-Code lesen und verstehe Struktur, `$state`, `{#if}`, `{#each}` und Props.

---

## Phase 2 — Daten & Zustand
*Ziel: Daten laden, zwischen Komponenten teilen, und Seiteneffekte kontrollieren.*
*Dauer: ~1 Woche*

### 2.1 Theorie lesen
- [ ] [[Svelte Stores]] — `writable`, `readable`, `derived`, `$`-Syntax
- [ ] [[Svelte Lifecycle]] — `onMount`, `onDestroy`
- [ ] [[Fetch in Svelte]] — HTTP-Requests, Fehlerbehandlung, AbortController

### 2.2 Interaktives Tutorial
- [ ] [Tutorial: Lifecycle](https://learn.svelte.dev/tutorial/onmount)
- [ ] [Tutorial: Stores](https://learn.svelte.dev/tutorial/writable-stores)

### 2.3 Übungsaufgaben
- [ ] **Übung E — onMount Fetch:** Lade beim Starten einer Komponente eine Liste von Daten von [jsonplaceholder.typicode.com/todos](https://jsonplaceholder.typicode.com/todos) und zeige sie an. Zeige während des Ladens einen "Lädt..."-Text.
- [ ] **Übung F — Ladezeichen + Fehler:** Erweitere Übung E: Zeige einen Fehler-Text wenn der Fetch fehlschlägt (teste mit einer falschen URL).
- [ ] **Übung G — Globaler Store:** Erstelle eine `stores.ts` mit einem `currentUser`-Writable-Store. Setze den User beim Klick auf einen "Login"-Button. Zeige den Namen des Users in einer separaten `Header.svelte`-Komponente an (ohne Props durchzureichen).
- [ ] **Übung H — Derived Store:** Erstelle einen `isLoggedIn`-Derived-Store basierend auf `currentUser`.

### 2.4 Checkpoint
- [ ] Ich kann Daten per Fetch laden, in `onMount` initialisieren, und globalen Zustand über Stores verwalten.

---

## Phase 3 — SvelteKit
*Ziel: Eine mehrseitige App mit SvelteKit aufbauen und verstehen wie das SAKE-Routing funktioniert.*
*Dauer: ~1 Woche*

### 3.1 Theorie lesen
- [ ] [[SvelteKit]] — Projektstruktur, `$lib`, Env-Variablen, Build-Befehle
- [ ] [[SvelteKit Routing]] — Datei-basiertes Routing, Layouts, `[param]`, `$page`-Store
- [ ] [[SvelteKit Load Functions]] — `+page.ts`, `$types`, Fehler und Redirects
- [ ] [[SvelteKit Static Adapter]] — `ssr = false`, `prerender`, Nginx, Dockerfile

### 3.2 Neues SvelteKit-Projekt erstellen
- [ ] `npx sv create sake-lern` im Terminal ausführen (TypeScript + Tailwind auswählen)
- [ ] `npm run dev` starten und die Start-App im Browser anschauen
- [ ] Projektstruktur mit dem Wiki-Eintrag vergleichen

### 3.3 Interaktives Tutorial
- [ ] [SvelteKit Tutorial](https://learn.svelte.dev/tutorial/kit-introduction) (Kapitel: Routing, Loading data)

### 3.4 Übungsaufgaben
- [ ] **Übung I — Zwei Seiten:** Erstelle in deinem Lernprojekt zwei Seiten: `/requester` und `/supporter`. Füge eine Navigation im Root-Layout hinzu.
- [ ] **Übung J — Layout:** Erstelle ein `+layout.svelte` mit einem Header (App-Name + aktive Navigation). Der aktive Link soll visuell hervorgehoben sein.
- [ ] **Übung K — Load Function:** Erstelle auf `/requester` eine `+page.ts` die simulierte Key-Daten zurückgibt (einfaches Array als Konstante). Zeige die Daten in der Seite an.
- [ ] **Übung L — Dynamische Route:** Erstelle eine Route `/keys/[keyId]` die den Parameter aus der URL liest und anzeigt.
- [ ] **Übung M — Static Adapter:** Füge `adapter-static` hinzu, setze `ssr = false` und `prerender = true`, und führe `npm run build` aus. Schau dir den `build/`-Ordner an.

### 3.5 Checkpoint
- [ ] Ich verstehe wie SAKE's `src/routes/`-Struktur zu URLs wird, was `+layout.svelte` macht, und warum `adapter-static` + `ssr = false` verwendet wird.

---

## Phase 4 — Projekt-Stack: TypeScript & Tailwind
*Ziel: Den SAKE-Code mit korrekten Typen lesen und eigene UI-Elemente stylen.*
*Dauer: ~0,5 Wochen*

### 4.1 Theorie lesen
- [ ] [[TypeScript in Svelte]] — Typen für Props, Runes, Events, `$types`
- [ ] [[Tailwind CSS]] — Utility-Klassen, Spacing, Flexbox, konditionale Klassen

### 4.2 Übungsaufgaben
- [ ] **Übung N — Typisierung:** Erstelle ein `src/lib/types.ts` in deinem Lernprojekt mit einem `ServiceAccountKey`-Interface (id, serviceAccount, expires, isExpired). Typisiere die Props von `KeyRow.svelte` damit.
- [ ] **Übung O — Tailwind Komponenten:** Baue mit Tailwind nach:
  - [ ] Einen Button (blau, hover-Effekt, disabled-State)
  - [ ] Eine Card mit Titel und Untertext
  - [ ] Ein Formularfeld mit Label und Fehlertext
  - [ ] Ein Status-Badge (grün = aktiv, rot = abgelaufen)
- [ ] **Übung P — Responsive:** Mache dein Layout so, dass die Navigation auf Mobilgeräten als vertikale Liste erscheint, auf Desktop als horizontale Leiste (`md:`-Prefix).

### 4.3 Checkpoint
- [ ] Ich kann TypeScript-Interfaces für meine Daten schreiben und eine Seite vollständig mit Tailwind stylen ohne eigenes CSS.

---

## Phase 5 — SAKE-Code lesen und verstehen
*Ziel: Den echten SAKE-Code navigieren und erste kleine Änderungen vornehmen.*
*Dauer: ~0,5 Wochen*

### 5.1 SAKE-Repo erkunden
- [ ] `src/routes/`-Ordner anschauen: welche Seiten gibt es?
- [ ] `src/lib/`-Ordner anschauen: welche Stores, Types, Components gibt es?
- [ ] `svelte.config.js` anschauen und mit [[SvelteKit Static Adapter]] vergleichen
- [ ] `package.json` anschauen: welche Dependencies werden verwendet?

### 5.2 Kernfluss nachvollziehen
- [ ] Die Requester-Flow-Seite im Code finden und lesen
- [ ] Wo wird `pkijs` / Web Crypto API verwendet? (→ `window.crypto.subtle`)
- [ ] Welcher Store hält den Zustand des laufenden Key-Requests?
- [ ] Wie wird das Zertifikat vom Browser an den Supporter-Link übergeben?

### 5.3 Erste Änderungen
- [ ] Einen Text auf einer Seite ändern und im Browser testen (`npm run dev`)
- [ ] Einen Button mit Tailwind-Klassen optisch anpassen
- [ ] Eine neue Seite `/info` mit Erklärungstext zum SAKE-Prozess erstellen

### 5.4 Checkpoint
- [ ] Ich kann den Requester-Flow im Code von Anfang bis Ende verfolgen und erklären welche Svelte-Konzepte wo verwendet werden.

---

## Fortschritt

| Phase | Status |
|---|---|
| Phase 1 — Grundlagen | Nicht begonnen |
| Phase 2 — Daten & Zustand | Nicht begonnen |
| Phase 3 — SvelteKit | Nicht begonnen |
| Phase 4 — TypeScript & Tailwind | Nicht begonnen |
| Phase 5 — SAKE-Code | Nicht begonnen |

---

## Hilfreiche Ressourcen

| Ressource | Wofür |
|---|---|
| [learn.svelte.dev](https://learn.svelte.dev) | Interaktives Tutorial (Svelte + SvelteKit) |
| [svelte.dev/docs](https://svelte.dev/docs) | Offizielle Svelte 5 Referenz |
| [kit.svelte.dev/docs](https://kit.svelte.dev/docs) | Offizielle SvelteKit 2 Referenz |
| [tailwindcss.com/docs](https://tailwindcss.com/docs) | Tailwind Referenz (alle Utility-Klassen) |
| [typescript-lang.org/docs](https://www.typescriptlang.org/docs/) | TypeScript Referenz |
| [svelte.dev/playground](https://svelte.dev/playground) | Svelte im Browser ausprobieren (kein Setup) |
