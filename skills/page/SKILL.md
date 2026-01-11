---
name: page
description: Erstellt oder bearbeitet statische Seiten fur Publii. Nutze fur Impressum, Uber uns, Kontakt etc.
---

# Statische Seite fur Publii erstellen

## Verfugbare MCP Tools

- `mcp__plugin_publii_publii__list_sites` - Sites auflisten
- `mcp__plugin_publii_publii__list_pages` - Pages auflisten
- `mcp__plugin_publii_publii__get_page` - Page lesen
- `mcp__plugin_publii_publii__create_page` - Page erstellen
- `mcp__plugin_publii_publii__update_page` - Page aktualisieren
- `mcp__plugin_publii_publii__delete_page` - Page loschen

## Ablauf

### 1. Site ermitteln

Prufe CLAUDE.md auf `publii_default_site`.

Falls nicht vorhanden:
- `mcp__plugin_publii_publii__list_sites` aufrufen
- User nach Auswahl fragen

### 2. Seiten-Typ klaren

Frage den User:
> "Mochtest du eine **neue Seite erstellen** oder eine **bestehende bearbeiten**?"

Bei bestehender Seite:
- `mcp__plugin_publii_publii__list_pages` aufrufen
- Liste anzeigen
- User wahlt aus
- `mcp__plugin_publii_publii__get_page` fur aktuellen Inhalt

### 3. Content erstellen

**HTML generieren, NICHT Markdown!**

Fur rechtliche Seiten (Impressum, Datenschutz):
- Frage nach allen notwendigen Angaben
- Verwende korrektes rechtliches Format
- Prufe auf Vollstandigkeit

Typische Seiten:
- **Impressum**: Name, Adresse, Kontakt, Vereinsregister
- **Datenschutz**: Verantwortlicher, Datenverarbeitung, Rechte
- **Uber uns**: Vereinsgeschichte, Team, Mission
- **Kontakt**: Anschrift, Telefon, E-Mail, Anfahrt

### 4. Speichern

Neue Seite:
```
mcp__plugin_publii_publii__create_page(
    title="...",
    content="<p>HTML Content</p>",
    status="draft",
)
```

Bestehende Seite:
```
mcp__plugin_publii_publii__update_page(
    page_id=42,
    content="<p>Neuer HTML Content</p>",
)
```

### 5. Bestatigung

> "Seite '[Titel]' gespeichert (ID: 42, Status: Draft).
>
> Offne Publii und klicke 'Sync' um die Anderungen zu veroffentlichen."

## Beispiel: Impressum erstellen

**User:** "Erstelle ein Impressum fur unseren Verein"

**Assistent:**

1. Fragt nach:
   - Vereinsname
   - Adresse
   - Vereinsregister-Nummer
   - Vertretungsberechtigte
   - Kontakt (Tel, E-Mail)

2. Generiert HTML:

```html
<h2>Angaben gemaess 5 TMG</h2>
<p>
  <strong>[Vereinsname]</strong><br>
  [Strasse Nr.]<br>
  [PLZ Ort]
</p>

<h2>Vertreten durch</h2>
<p>[Vorstand Name]</p>

<h2>Kontakt</h2>
<p>
  Telefon: [Nummer]<br>
  E-Mail: [Adresse]
</p>

<h2>Registereintrag</h2>
<p>
  Eingetragen im Vereinsregister.<br>
  Registergericht: [Gericht]<br>
  Registernummer: [Nummer]
</p>
```

3. Speichert als Draft
4. Zeigt Bestatigung
