# publii - Claude Code Plugin

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/valgard/publii-claude-code)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

Claude Code Plugin für Publii CMS Integration. Ermöglicht das Erstellen und Bearbeiten von Blog-Posts und statischen Seiten direkt aus Claude Code.

## Quick-Start

```bash
# 1. publii-mcp Server klonen und einrichten
git clone https://github.com/valgard/publii-mcp.git
cd publii-mcp
uv sync

# 2. Plugin installieren
claude plugins add /path/to/publii-claude-code

# 3. Ersten Post erstellen
/publii:post "Mein erster Post"
```

## Installation

```bash
# Plugin zu Claude Code hinzufügen
claude plugins add /path/to/publii-claude-code
```

## Voraussetzungen

- **Claude Code** CLI installiert
- **publii-mcp** Server eingerichtet (siehe MCP-Server Setup)
- **Publii CMS** mit mindestens einer Site
- **uv** Package Manager für Python

## MCP-Server Setup

Der `publii-mcp` Server ist erforderlich für die Kommunikation mit Publii:

```bash
# Repository klonen
git clone https://github.com/valgard/publii-mcp.git

# In das Verzeichnis wechseln
cd publii-mcp

# Abhängigkeiten installieren
uv sync

# Server testen
uv run publii-mcp serve
```

Die `.mcp.json` im Plugin-Verzeichnis konfiguriert den Server automatisch.

## Skills

### /publii:post

Erstellt Blog-Posts für Publii mit verschiedenen Modi:

| Modus | Beschreibung |
|-------|--------------|
| Interaktiv | Geführte Erstellung mit Fragen |
| Prompt-basiert | Direktes Erstellen aus Thema |
| Template-basiert | Vordefinierte Strukturen nutzen |
| Review-basiert | Vorschau vor Veröffentlichung |

```bash
# Interaktiver Modus
/publii:post

# Mit Thema
/publii:post "Wettkampfbericht Leipzig"

# Mit Template
/publii:post --template wettkampfbericht
```

**Verfügbare Templates:**
- `wettkampfbericht` - Turnier- und Wettkampfberichte
- `vereinsnews` - Allgemeine Vereinsnachrichten
- `trainingsinfo` - Trainingsankündigungen

### /publii:page

Erstellt oder bearbeitet statische Seiten (Impressum, Datenschutz, etc.).

```bash
# Interaktiver Modus
/publii:page

# Mit Titel
/publii:page "Impressum"
```

## Konfiguration

In der Projekt-CLAUDE.md können Standardwerte definiert werden:

```markdown
## Publii Konfiguration

publii_default_site: meine-site
publii_default_author: 1
```

## Troubleshooting

### MCP-Server startet nicht

```bash
# Prüfen ob uv installiert ist
uv --version

# Abhängigkeiten neu installieren
cd /path/to/publii-mcp
uv sync --reinstall
```

### Site wird nicht gefunden

1. Publii öffnen und prüfen ob Site existiert
2. Site-Slug in CLAUDE.md prüfen (lowercase, keine Leerzeichen)
3. `list_sites` Tool testen: Das Plugin listet verfügbare Sites

### Post/Page wird nicht erstellt

- Prüfen ob `publii_default_author` eine gültige Author-ID ist
- Status prüfen: `draft` (Standard) oder `published`
- Publii muss für Veröffentlichung synchronisiert werden

Weitere Troubleshooting-Informationen: [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)

## Lizenz

MIT License - siehe [LICENSE](LICENSE)
