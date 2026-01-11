---
name: post
description: Erstellt Blog-Posts für Publii. Nutze wenn der User einen Blog-Post, Artikel oder News schreiben möchte.
---

# Blog-Post für Publii erstellen

## Verfügbare MCP Tools

Die folgenden Tools sind über den publii MCP Server verfügbar:

- `mcp__plugin_publii_publii__list_sites` - Sites auflisten
- `mcp__plugin_publii_publii__list_posts` - Posts auflisten
- `mcp__plugin_publii_publii__get_post` - Post lesen
- `mcp__plugin_publii_publii__create_post` - Post erstellen
- `mcp__plugin_publii_publii__update_post` - Post aktualisieren
- `mcp__plugin_publii_publii__list_tags` - Tags auflisten
- `mcp__plugin_publii_publii__list_authors` - Autoren auflisten

## Workflow-Modi

Bestimme den Modus anhand des User-Inputs:

### 1. Prompt-basiert
User gibt Thema oder Stichworte.
→ Direkt Post schreiben basierend auf dem Input.

### 2. Interaktiv
User gibt nur `/publii:post` ohne Argumente.
→ Frage nacheinander nach:
1. Site (wenn nicht in CLAUDE.md konfiguriert)
2. Thema/Titel
3. Zielgruppe
4. Tonalität (formell/informell)
5. Gewünschte Länge

### 3. Template-basiert
User erwähnt einen Template-Typ (z.B. "Wettkampfbericht").
→ Lade das passende Template aus `templates/` und frage nach den spezifischen Daten.

### 4. Halb-automatisch (Review)
User möchte den Post vor dem Speichern sehen.
→ Generiere Entwurf, zeige Vorschau, warte auf Freigabe.

## Ablauf

### 1. Site ermitteln

Prüfe zuerst CLAUDE.md auf `publii_default_site`:

```
## Publii
publii_default_site: meine-site
```

Falls nicht vorhanden:
- Rufe `mcp__plugin_publii_publii__list_sites` auf
- Zeige verfügbare Sites
- Frage User nach Auswahl

### 2. Content erstellen

**WICHTIG: HTML generieren, NICHT Markdown!**

Nutze semantische HTML-Tags:
- `<h2>`, `<h3>` für Überschriften (KEINE `<h1>` - ist der Titel!)
- `<p>` für Absätze
- `<ul>`, `<ol>` für Listen
- `<strong>`, `<em>` für Hervorhebungen
- `<blockquote>` für Zitate

**KEINE** `#DOMAIN_NAME#` Platzhalter verwenden - das wird von Publii gehandhabt.

### 3. Veröffentlichung

Frage den User:
> "Soll der Post als **Draft** (Entwurf) oder direkt als **Published** gespeichert werden?"

Standard ist `draft` - sicherer für Review.

Rufe dann auf:

```
mcp__plugin_publii_publii__create_post(
    title="...",
    content="<p>HTML Content</p>",
    site="...",  # Falls nicht Default
    status="draft",  # oder "published"
)
```

### 4. Bestätigung

Nach erfolgreicher Erstellung:

> "Post erstellt (ID: 42, Status: Draft).
>
> **Nächste Schritte:**
> 1. Öffne die Publii App
> 2. Wähle deine Site
> 3. Prüfe den Post im Editor
> 4. Klicke 'Sync' um die Änderungen zu veröffentlichen"

## Templates

Verfügbare Templates in `templates/`:

| Template | Verwendung |
|----------|------------|
| `wettkampfbericht.md` | Für Wettkampfberichte und Turniere |
| `vereinsnews.md` | Für allgemeine Vereinsnachrichten |
| `trainingsinfo.md` | Für Trainingshinweise und Termine |

Wenn ein Template verwendet wird, lies es mit dem Read-Tool und folge den Anweisungen darin.

## HTML-Beispiel: Vereinsnews

**Input:** "Schreib einen Post über die neue Vereinskleidung"

**Generiertes HTML:**

```html
<h2>Neue Vereinskleidung ab sofort erhältlich</h2>
<p>
  Wir freuen uns, euch unsere neue Kollektion präsentieren zu können!
  Ab sofort könnt ihr die neuen Trikots und Trainingsanzüge bestellen.
</p>

<h2>Das Sortiment</h2>
<ul>
  <li><strong>Wettkampf-Trikot</strong>: Blau/Weiß, atmungsaktiv (45€)</li>
  <li><strong>Trainingsanzug</strong>: Dunkelblau mit Vereinslogo (75€)</li>
  <li><strong>Sporttasche</strong>: Mit Schuhfach und Nassfach (35€)</li>
</ul>

<h2>So bestellt ihr</h2>
<p>
  Bestellungen nimmt <strong>Max Mustermann</strong> bis zum
  <em>30. Januar</em> entgegen. Zahlung bei Abholung im Vereinsheim.
</p>
<p>
  Bei Fragen wendet euch an
  <a href="mailto:kleidung@verein.de">kleidung@verein.de</a>.
</p>
```

## HTML-Beispiel: Wettkampfbericht

**Input:** "Wettkampfbericht vom Stadtpokal"

**Generiertes HTML:**

```html
<h2>Erfolgreicher Auftritt beim Stadtpokal</h2>
<p>
  Am vergangenen Samstag, dem 18. Januar, nahmen 12 Sportlerinnen und
  Sportler unseres Vereins am diesjährigen Stadtpokal teil. Bei
  hervorragenden Bedingungen konnten wir uns über mehrere Podestplätze freuen.
</p>

<h2>Unsere Ergebnisse</h2>
<ul>
  <li><strong>Anna Schmidt</strong>: 1. Platz Jugend weiblich</li>
  <li><strong>Tom Müller</strong>: 2. Platz Jugend männlich</li>
  <li><strong>Lisa Weber</strong>: 3. Platz Schüler weiblich</li>
</ul>

<h2>Highlight des Tages</h2>
<blockquote>
  "Anna zeigte eine fehlerfreie Vorstellung und setzte sich souverän
  gegen die starke Konkurrenz durch."
</blockquote>
<p><em>— Trainer Peter Hoffmann</em></p>

<h2>Ausblick</h2>
<p>
  Der nächste Wettkampf steht am <strong>15. Februar</strong> an:
  die Bezirksmeisterschaften in Dresden. Wir danken allen Eltern
  für die Unterstützung und Anfeuerung!
</p>
```

## Beispiel-Workflow

**User:** "Schreib einen Post über unseren Wettkampf in Leipzig"

**Assistent:**
1. Prüft CLAUDE.md → findet `publii_default_site`
2. Erkennt "Wettkampf" → Template `wettkampfbericht.md`
3. Fragt nach: Datum, Teilnehmer, Platzierungen, Highlights
4. Generiert HTML-Content (wie im Beispiel oben)
5. `create_post(title="Wettkampfbericht Leipzig", content="...", status="draft")`
6. Zeigt Bestätigung mit nächsten Schritten
