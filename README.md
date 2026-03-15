# Amateurfunk Trainer — Klasse A

Prüfungsvorbereitung für die deutsche Amateurfunkprüfung Klasse A nach BNetzA-Vorgaben.

Entwickelt von [Claude](https://claude.ai) (Anthropic) in Zusammenarbeit mit Felix F.

## Features

- **Lernmodus** mit gewichtetem, garantiertem Durchlauf aller 1750 Fragen
  - Schwächen werden bevorzugt, jede Frage kommt garantiert dran
  - `correct`-Zähler steigt bei richtiger Antwort, sinkt bei falscher (min. 0)
  - Beherrscht = mindestens 2× richtig + zuletzt richtig
- **Prüfungssimulation** exakt nach BNetzA-Vorgaben:
  - 5 separate Teile: Technik N / E / A (je 25 Fr.), Betrieb (25 Fr.), Vorschriften (25 Fr.)
  - Korrekte Zeitlimits: 45 / 45 / 60 / 45 / 45 Minuten
  - Gesamtdauer: 240 Minuten
  - Übergangs-Karte beim Wechsel zwischen Prüfungsteilen
  - Kein Feedback während der Prüfung — Auswertung am Ende
  - Falsche Fragen nach Prüfung einzeln einsehbar mit richtiger Antwort
- **Statistik nach Themenbereich** — Antippen des Fortschrittsbalkens im Dashboard
- **Hilfsmittel** — offizielles BNetzA-Dokument (Formelsammlung, Frequenzplan, Rufzeichenplan) jederzeit über Topbar-Button abrufbar
- **SVG-Schaltpläne** und LaTeX-Formeln (lazy geladen)
- **Zwei Modi:** mit Account (Cloud-Sync über Supabase) oder ohne Account (lokal)
- **PWA** — installierbar auf PC und Handy, läuft wie eine native App

## Online nutzen

**[https://felixfendt04-sketch.github.io/afu_trainer/](https://felixfendt04-sketch.github.io/afu_trainer/)**

> **Hinweis:** Diese Instanz ist für den persönlichen Gebrauch des Autors eingerichtet (Login erforderlich).
> Der **Gastmodus** ist ohne Account vollständig nutzbar — Lernstand wird lokal im Browser gespeichert.
> Für eigene Cloud-Sync-Funktionalität bitte eine eigene Instanz hosten (siehe unten).

## Lokal nutzen (Windows)

1. Repository klonen oder ZIP herunterladen
2. `starten.bat` ausführen — startet lokalen Server und öffnet Browser automatisch
3. Setzt Python voraus (`python.org/downloads`, "Add to PATH" anhaken)

Im lokalen Betrieb läuft die App vollständig im Gastmodus — Lernstand wird lokal gespeichert.

## Eigene Instanz hosten (mit Cloud-Sync)

1. **Repository forken** auf GitHub
2. **Supabase-Projekt anlegen** auf `supabase.com`
3. **Tabelle anlegen** (SQL Editor):
```sql
create table lernstand (
  id uuid references auth.users(id) primary key,
  data jsonb not null,
  updated_at timestamptz default now()
);
alter table lernstand enable row level security;
create policy "Nur eigene Daten" on lernstand
  for all using (auth.uid() = id) with check (auth.uid() = id);
```
4. **User anlegen**: Authentication → Users → Add user
5. **Sign-ups deaktivieren**: Authentication → Providers → Email → Disable sign ups
6. **GitHub Secrets setzen**: Settings → Secrets → Actions:
   - `SUPABASE_URL` = deine Projekt-URL
   - `SUPABASE_KEY` = dein anon Key
7. **GitHub Pages aktivieren**: Settings → Pages → Branch `gh-pages`

GitHub Actions deployt bei jedem Push auf `main` automatisch auf `gh-pages`
und injiziert dabei die Supabase-Credentials sicher aus den Secrets.

## Technische Architektur

```
GitHub Pages          Supabase
(App-Hosting)    ←→   (Lernstand-Sync)
     ↕
Browser (Client)
— lädt Katalog + SVGs von GitHub Pages
— speichert/liest Lernstand via Supabase REST API
— läuft vollständig im Browser, kein eigener Server
```

## Lizenzhinweise

### App-Code
MIT License — Copyright (c) 2025 — Entwickelt mit Claude (Anthropic)

### Prüfungsfragen
Die Prüfungsfragen stammen von der Bundesnetzagentur und werden unter der
Datenlizenz Deutschland – Namensnennung – Version 2.0 bereitgestellt.

**Quellenangabe:**
Prüfungsfragen zum Erwerb von Amateurfunkprüfungsbescheinigungen,
Bundesnetzagentur, 3. Auflage, März 2024,
www.bundesnetzagentur.de/amateurfunk,
Datenlizenz Deutschland – Namensnennung – Version 2.0
(www.govdata.de/dl-de/by-2-0)

**Hinweis:** Die Prüfungsfragen wurden für die Verwendung in dieser
Web-Applikation technisch bearbeitet (JSON-Format, SVG-Einbettung,
LaTeX-Rendering). Der inhaltliche Fragenkatalog wurde nicht verändert.

### Hilfsmittel
Das Dokument `hilfsmittel.pdf` stammt von der Bundesnetzagentur, Referat 225,
Außenstelle Dortmund (Stand: 12.06.2024) und enthält die offiziellen
Prüfungshilfsmittel (Formelsammlung, AFuV Anlage 1, Rufzeichenplan).
