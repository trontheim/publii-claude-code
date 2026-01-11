# publii - Claude Code Plugin

Claude Code Plugin fur Publii CMS Integration. Ermoglicht das Erstellen und Bearbeiten von Blog-Posts und statischen Seiten direkt aus Claude Code.

## Installation

```bash
# Plugin zu Claude Code hinzufugen
claude plugins add /path/to/publii-claude-code
```

## Voraussetzungen

- **publii-mcp** Server muss installiert sein
- Publii CMS mit mindestens einer Site

## Skills

### /publii:post

Erstellt Blog-Posts fur Publii.

```bash
# Interaktiver Modus
/publii:post

# Mit Thema
/publii:post "Wettkampfbericht Leipzig"

# Mit Template
/publii:post --template wettkampfbericht
```

### /publii:page

Erstellt oder bearbeitet statische Seiten.

```bash
# Interaktiver Modus
/publii:page

# Mit Titel
/publii:page "Impressum"
```

## Konfiguration

In der Projekt-CLAUDE.md:

```markdown
## Publii

publii_default_site: meine-site
publii_default_author: 1
```
