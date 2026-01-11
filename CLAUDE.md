# Publii Claude Code Plugin

## Projekt

Claude Code Plugin für Publii CMS Integration (v1.0.0). Ermöglicht das Erstellen und Bearbeiten von Blog-Posts und statischen Seiten.

## Publii Konfiguration

```
publii_default_site: <site-slug>
publii_default_author: 1
```

Diese Werte werden von den Skills `/publii:post` und `/publii:page` als Standardwerte verwendet.

## Entwicklung

### Code-Konventionen

- **Sprache**: Alle Dokumentation und Skills in Deutsch
- **HTML-Generierung**: Semantisches HTML5, kein `<h1>` (reserviert für Titel)
- **Skill-Format**: YAML Front Matter + Markdown

### MCP-Tools Namensschema

Alle Tools folgen dem Muster: `mcp__plugin_publii_publii__<action>`

### Templates

Neue Templates unter `skills/post/templates/` anlegen mit Struktur:
- `## Benötigte Informationen`
- `## HTML-Struktur`
- `## Beispiel-Fragen`
