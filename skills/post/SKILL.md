---
name: post
description: Erstellt Blog-Posts fur Publii. Nutze wenn der User einen Blog-Post, Artikel oder News schreiben mochte.
---

# Blog-Post fur Publii erstellen

## Verfugbare MCP Tools

Die folgenden Tools sind uber den publii MCP Server verfugbar:

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
4. Tonalitat (formell/informell)
5. Gewunschte Lange

### 3. Template-basiert
User erwahnt einen Template-Typ (z.B. "Wettkampfbericht").
→ Lade das passende Template aus `templates/` und frage nach den spezifischen Daten.

### 4. Halb-automatisch (Review)
User mochte den Post vor dem Speichern sehen.
→ Generiere Entwurf, zeige Vorschau, warte auf Freigabe.

## Ablauf

### 1. Site ermitteln

Prufe zuerst CLAUDE.md auf `publii_default_site`:

```
## Publii
publii_default_site: meine-site
```

Falls nicht vorhanden:
- Rufe `mcp__plugin_publii_publii__list_sites` auf
- Zeige verfugbare Sites
- Frage User nach Auswahl

### 2. Content erstellen

**WICHTIG: HTML generieren, NICHT Markdown!**

Nutze semantische HTML-Tags:
- `<h2>`, `<h3>` fur Uberschriften (KEINE `<h1>` - ist der Titel!)
- `<p>` fur Absatze
- `<ul>`, `<ol>` fur Listen
- `<strong>`, `<em>` fur Hervorhebungen
- `<blockquote>` fur Zitate

**KEINE** `#DOMAIN_NAME#` Platzhalter verwenden - das wird von Publii gehandhabt.

### 3. Veroffentlichung

Frage den User:
> "Soll der Post als **Draft** (Entwurf) oder direkt als **Published** gespeichert werden?"

Standard ist `draft` - sicherer fur Review.

Rufe dann auf:

```
mcp__plugin_publii_publii__create_post(
    title="...",
    content="<p>HTML Content</p>",
    site="...",  # Falls nicht Default
    status="draft",  # oder "published"
)
```

### 4. Bestatigung

Nach erfolgreicher Erstellung:

> "Post erstellt (ID: 42, Status: Draft).
>
> **Nachste Schritte:**
> 1. Offne die Publii App
> 2. Wahle deine Site
> 3. Prufe den Post im Editor
> 4. Klicke 'Sync' um die Anderungen zu veroffentlichen"

## Templates

Verfugbare Templates in `templates/`:

| Template | Verwendung |
|----------|------------|
| `wettkampfbericht.md` | Fur Wettkampfberichte und Turniere |
| `vereinsnews.md` | Fur allgemeine Vereinsnachrichten |
| `trainingsinfo.md` | Fur Trainingshinweise und Termine |

Wenn ein Template verwendet wird, lies es mit dem Read-Tool und folge den Anweisungen darin.

## Beispiel-Workflow

**User:** "Schreib einen Post uber unseren Wettkampf in Leipzig"

**Assistent:**
1. Pruft CLAUDE.md → findet `publii_default_site`
2. Erkennt "Wettkampf" → Template `wettkampfbericht.md`
3. Fragt nach: Datum, Teilnehmer, Platzierungen, Highlights
4. Generiert HTML-Content
5. `create_post(title="Wettkampfbericht Leipzig", content="...", status="draft")`
6. Zeigt Bestatigung mit nachsten Schritten
