# Amateurfunk Trainer — Klasse A

Prüfungsvorbereitung für die deutsche Amateurfunkprüfung Klasse A nach BNetzA-Vorgaben.

## Features

- Lernmodus mit gewichtetem, garantiertem Durchlauf aller 1750 Fragen
- Schwächen werden bevorzugt, aber jede Frage kommt dran
- Prüfungssimulation exakt nach BNetzA (75/25/25 Fragen, echte Zeitlimits)
- Auswertung pro Prüfungsteil mit Bestehensgrenze 75%
- SVG-Schaltpläne und LaTeX-Formeln (lazy geladen)
- Zwei Modi: mit Account (Cloud-Sync) oder ohne Account (lokal)
- PWA — installierbar auf PC und Handy

## Nutzung

### Online (empfohlen)
Die App läuft direkt im Browser — kein Download nötig.

### Lokal (Windows)
1. Repository klonen oder ZIP herunterladen
2. `starten.bat` ausführen — startet lokalen Server und öffnet Browser
3. Setzt Python voraus (`python.org/downloads`, "Add to PATH" anhaken)

## Für eigene Instanz (selbst hosten)

Du möchtest die App mit eigenem Cloud-Sync nutzen? So geht's:

1. **Supabase-Projekt anlegen** auf `supabase.com`
2. **Tabelle anlegen** (SQL Editor):
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
3. **User anlegen**: Authentication → Users → Add user
4. **Sign-ups deaktivieren**: Authentication → Providers → Email → Disable sign ups
5. **Repository forken**
6. **GitHub Secrets setzen**: Settings → Secrets → Actions:
   - `SUPABASE_URL` = deine Projekt-URL
   - `SUPABASE_KEY` = dein anon Key
7. GitHub Actions deployt automatisch auf GitHub Pages

## Lizenzhinweise

### App-Code
MIT License — Copyright (c) 2025

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
