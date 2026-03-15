# Amateurfunk Trainer — Klasse A

Prüfungsvorbereitung für die deutsche Amateurfunkprüfung Klasse A nach BNetzA-Vorgaben.

Entwickelt von [Claude](https://claude.ai) (Anthropic) in Zusammenarbeit mit Felix F.

## Features

- Lernmodus mit gewichtetem, garantiertem Durchlauf aller 1750 Fragen
- Schwächen werden bevorzugt, aber jede Frage kommt dran
- Prüfungssimulation exakt nach BNetzA (75/25/25 Fragen, echte Zeitlimits)
- Auswertung pro Prüfungsteil mit Bestehensgrenze 75%
- Falsche Fragen nach Prüfung einzeln einsehbar mit richtiger Antwort
- SVG-Schaltpläne und LaTeX-Formeln (lazy geladen)
- Zwei Modi: mit Account (Cloud-Sync) oder ohne Account (lokal)
- PWA — installierbar auf PC und Handy

## Online nutzen

Die App läuft direkt im Browser — kein Download nötig:
**[https://felixfendt04-sketch.github.io/afu_trainer/](https://felixfendt04-sketch.github.io/afu_trainer/)**

> **Hinweis:** Diese Instanz ist für den persönlichen Gebrauch des Autors eingerichtet.
> Für eigene Cloud-Sync-Funktionalität bitte eine eigene Instanz hosten (siehe unten).
> Der Gastmodus ist ohne Account vollständig nutzbar — der Lernstand wird dann lokal im Browser gespeichert.

## Lokal nutzen (Windows)

1. Repository klonen oder ZIP herunterladen
2. `starten.bat` ausführen — startet lokalen Server und öffnet Browser
3. Setzt Python voraus (`python.org/downloads`, "Add to PATH" anhaken)

Im lokalen Betrieb läuft die App vollständig ohne Account — Lernstand wird lokal gespeichert.

## Eigene Instanz hosten (mit Cloud-Sync)

Für eigene Cloud-Sync-Funktionalität musst du die App selbst hosten:

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

GitHub Actions deployt bei jedem Push auf `main` automatisch auf GitHub Pages
und injiziert dabei die Supabase-Credentials aus den Secrets.

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
