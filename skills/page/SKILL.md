---
name: page
description: Erstellt oder bearbeitet statische Seiten für Publii. Nutze für Impressum, Über uns, Kontakt etc.
---

# Statische Seite für Publii erstellen

## Verfügbare MCP Tools

- `mcp__plugin_publii_publii__list_sites` - Sites auflisten
- `mcp__plugin_publii_publii__list_pages` - Pages auflisten
- `mcp__plugin_publii_publii__get_page` - Page lesen
- `mcp__plugin_publii_publii__create_page` - Page erstellen
- `mcp__plugin_publii_publii__update_page` - Page aktualisieren
- `mcp__plugin_publii_publii__delete_page` - Page löschen

## Ablauf

### 1. Site ermitteln

Prüfe CLAUDE.md auf `publii_default_site`.

Falls nicht vorhanden:
- `mcp__plugin_publii_publii__list_sites` aufrufen
- User nach Auswahl fragen

### 2. Seiten-Typ klären

Frage den User:
> "Möchtest du eine **neue Seite erstellen** oder eine **bestehende bearbeiten**?"

Bei bestehender Seite:
- `mcp__plugin_publii_publii__list_pages` aufrufen
- Liste anzeigen
- User wählt aus
- `mcp__plugin_publii_publii__get_page` für aktuellen Inhalt

### 3. Content erstellen

**HTML generieren, NICHT Markdown!**

Für rechtliche Seiten (Impressum, Datenschutz):
- Frage nach allen notwendigen Angaben
- Verwende korrektes rechtliches Format
- Prüfe auf Vollständigkeit

Typische Seiten:
- **Impressum**: Name, Adresse, Kontakt, Vereinsregister
- **Datenschutz**: Verantwortlicher, Datenverarbeitung, Rechte
- **Über uns**: Vereinsgeschichte, Team, Mission
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

### 5. Bestätigung

> "Seite '[Titel]' gespeichert (ID: 42, Status: Draft).
>
> Öffne Publii und klicke 'Sync' um die Änderungen zu veröffentlichen."

## Beispiel: Impressum erstellen

**User:** "Erstelle ein Impressum für unseren Verein"

**Assistent:**

1. Fragt nach:
   - Vereinsname
   - Adresse
   - Vereinsregister-Nummer
   - Vertretungsberechtigte
   - Kontakt (Tel, E-Mail)

2. Generiert HTML:

```html
<h2>Angaben gemäß § 5 TMG</h2>
<p>
  <strong>[Vereinsname]</strong><br>
  [Straße Nr.]<br>
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
4. Zeigt Bestätigung
