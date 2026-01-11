# Troubleshooting

Lösungen für häufige Probleme mit dem Publii Claude Code Plugin.

## MCP-Server Probleme

### Server startet nicht

**Symptom:** Fehlermeldung beim Starten von `publii-mcp serve`

**Lösungen:**

1. **uv nicht installiert**
   ```bash
   # uv installieren (macOS/Linux)
   curl -LsSf https://astral.sh/uv/install.sh | sh

   # Version prüfen
   uv --version
   ```

2. **Abhängigkeiten fehlen**
   ```bash
   cd /path/to/publii-mcp
   uv sync --reinstall
   ```

3. **Python-Version inkompatibel**
   ```bash
   # Python-Version prüfen (mindestens 3.10 erforderlich)
   python --version

   # Mit asdf Python-Version setzen
   asdf install python 3.12.0
   asdf local python 3.12.0
   ```

### Server läuft, aber Tools funktionieren nicht

**Symptom:** MCP-Tools geben Fehler zurück oder sind nicht verfügbar

**Lösungen:**

1. **`.mcp.json` prüfen**
   ```json
   {
     "mcpServers": {
       "publii": {
         "command": "uv",
         "args": ["run", "publii-mcp", "serve"],
         "cwd": "/absolute/path/to/publii-mcp"
       }
     }
   }
   ```

2. **Pfad muss absolut sein** - Relative Pfade funktionieren nicht

3. **Claude Code neu starten** nach Änderungen an `.mcp.json`

## Site-Probleme

### Site wird nicht gefunden

**Symptom:** `list_sites` gibt leere Liste zurück oder Site fehlt

**Lösungen:**

1. **Publii Sites-Verzeichnis prüfen**
   - macOS: `~/Documents/Publii/sites/`
   - Windows: `%USERPROFILE%\Documents\Publii\sites\`
   - Linux: `~/Documents/Publii/sites/`

2. **Site in Publii öffnen** - Site muss mindestens einmal in Publii geöffnet worden sein

3. **Site-Slug prüfen**
   - Nur Kleinbuchstaben
   - Keine Leerzeichen (Bindestriche erlaubt)
   - Entspricht dem Ordnernamen unter `sites/`

### Falsche Site wird verwendet

**Symptom:** Posts/Pages werden in falscher Site erstellt

**Lösung:** `publii_default_site` in CLAUDE.md prüfen:
```markdown
## Publii Konfiguration

publii_default_site: richtige-site
```

## Post/Page Probleme

### Post wird nicht erstellt

**Symptom:** `create_post` gibt Fehler zurück

**Mögliche Ursachen:**

1. **Ungültige Author-ID**
   ```bash
   # Verfügbare Autoren auflisten
   # Nutze list_authors Tool
   ```

2. **Fehlende Pflichtfelder**
   - `title` ist erforderlich
   - `content` darf nicht leer sein

3. **Ungültiger Status**
   - Nur `draft` oder `published` erlaubt

### Post erscheint nicht auf Website

**Symptom:** Post erstellt, aber nicht sichtbar

**Lösungen:**

1. **Status prüfen** - `draft` Posts sind nicht öffentlich

2. **In Publii synchronisieren**
   - Publii öffnen
   - "Sync your website" klicken
   - Auf Deployment warten

3. **Cache leeren** - Browser-Cache oder CDN-Cache

### HTML wird nicht korrekt dargestellt

**Symptom:** HTML-Tags werden als Text angezeigt

**Lösung:** Publii Editor-Modus prüfen
- Posts müssen im "HTML" oder "Block Editor" Modus sein
- WYSIWYG-Editor kann HTML-Struktur verändern

## Template-Probleme

### Template wird nicht gefunden

**Symptom:** `--template` Parameter funktioniert nicht

**Lösung:** Template-Pfad prüfen
```
skills/post/templates/
├── wettkampfbericht.md
├── vereinsnews.md
└── trainingsinfo.md
```

### Template-Struktur wird nicht eingehalten

**Symptom:** Generierter Post hat falsche Struktur

**Lösung:** Template-Datei prüfen - muss folgende Abschnitte haben:
- `## Benötigte Informationen`
- `## HTML-Struktur`
- `## Beispiel-Fragen`

## Berechtigungsprobleme

### Zugriff verweigert

**Symptom:** Fehler beim Lesen/Schreiben von Dateien

**Lösungen:**

1. **Publii-Verzeichnis Berechtigungen**
   ```bash
   # macOS/Linux
   chmod -R 755 ~/Documents/Publii
   ```

2. **Plugin-Verzeichnis Berechtigungen**
   ```bash
   chmod -R 755 /path/to/publii-claude-code
   ```

## Debugging

### Verbose Logging aktivieren

```bash
# MCP-Server mit Debug-Output starten
DEBUG=* uv run publii-mcp serve
```

### Tool-Aufrufe testen

Direkt in Claude Code einzelne Tools testen:
```
Nutze das Tool mcp__plugin_publii_publii__list_sites
```

### Log-Dateien

- Claude Code Logs: `~/.claude/logs/`
- MCP-Server Logs: Ausgabe im Terminal

## Support

Bei weiteren Problemen:
1. [GitHub Issues](https://github.com/valgard/publii-claude-code/issues) prüfen
2. Neues Issue mit folgenden Informationen erstellen:
   - Betriebssystem und Version
   - Claude Code Version
   - Fehlermeldung (vollständig)
   - Schritte zum Reproduzieren
