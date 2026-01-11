# Architektur

Technische Übersicht und Design-Entscheidungen des Publii Claude Code Plugins.

## Systemübersicht

### Zwei-Repository-Architektur

Das Publii-Integration besteht aus zwei separaten Repositories:

| Repository | Inhalt | Zweck |
|------------|--------|-------|
| **publii-mcp** | MCP-Server (Python) + `.mcp.json` | Stellt Tools bereit |
| **publii-claude-code** | Skills + Templates | Definiert Workflows |

```
┌─────────────────────────────────────────────────────────────┐
│                      Claude Code CLI                         │
├──────────────────────────────┬──────────────────────────────┤
│    publii-claude-code        │         publii-mcp           │
│    (Skills Repository)       │    (MCP-Server Repository)   │
│  ┌────────────────────────┐  │  ┌────────────────────────┐  │
│  │  /publii:post          │  │  │  MCP-Server (Python)   │  │
│  │  /publii:page          │  │  │  .mcp.json             │  │
│  │  Templates             │  │  │  SQLite-Operationen    │  │
│  └────────────────────────┘  │  └────────────────────────┘  │
├──────────────────────────────┴──────────────────────────────┤
│                    MCP Protocol Layer                        │
│              mcp__plugin_publii_publii__*                    │
├─────────────────────────────────────────────────────────────┤
│                     Publii CMS                               │
│              ~/Documents/Publii/sites/                       │
└─────────────────────────────────────────────────────────────┘
```

**Warum zwei Repositories?**
1. **Trennung von Concerns** - Server-Logik (Python) getrennt von Workflow-Definition (Markdown)
2. **Unabhängige Versionierung** - MCP-Server kann aktualisiert werden ohne Skills zu ändern
3. **Wiederverwendbarkeit** - Skills können mit anderen MCP-Servern kombiniert werden

## Design-Entscheidungen

### Warum separate Post/Page Skills?

**Entscheidung:** Zwei spezialisierte Skills statt einem generischen "content" Skill.

**Begründung:**
1. **Unterschiedliche Workflows** - Posts haben Tags, Autoren, Templates; Pages sind einfacher
2. **Klarere Benutzerführung** - `/publii:post` vs `/publii:page` ist intuitiver als Parameter
3. **Bessere Wartbarkeit** - Änderungen an Posts betreffen nicht die Page-Logik

### Warum HTML statt Markdown?

**Entscheidung:** Skills generieren HTML direkt, nicht Markdown.

**Begründung:**
1. **Publii-Kompatibilität** - Publii speichert Inhalte intern als HTML
2. **Volle Kontrolle** - Semantische HTML5-Tags (`<article>`, `<section>`) werden korrekt erhalten
3. **WYSIWYG-Sicherheit** - Markdown-zu-HTML-Konvertierung kann unerwartete Ergebnisse liefern

**Konsequenz:**
- Kein `<h1>` in Content (wird für Post-Titel reserviert)
- `<h2>` bis `<h4>` für Abschnittsüberschriften
- Semantische Tags: `<strong>`, `<em>` statt `<b>`, `<i>`

### Warum separate Template-Dateien?

**Entscheidung:** Templates als eigenständige Markdown-Dateien unter `skills/post/templates/`.

**Begründung:**
1. **Erweiterbarkeit** - Neue Templates ohne SKILL.md-Änderung hinzufügen
2. **Lesbarkeit** - Template-Struktur übersichtlich dokumentiert
3. **Wiederverwendbarkeit** - Templates können zwischen Projekten geteilt werden

**Struktur jedes Templates:**
```markdown
# Template: [Name]

## Benötigte Informationen
[Was der Skill vom User erfragen muss]

## HTML-Struktur
[Das HTML-Gerüst mit Platzhaltern]

## Beispiel-Fragen
[Formulierungsvorschläge für den Skill]
```

## Komponenten

### Plugin-Konfiguration

**Datei:** `.claude-plugin/plugin.json`

```json
{
  "name": "publii",
  "version": "1.0.0",
  "description": "Publii CMS Integration"
}
```

**Zweck:** Identifiziert das Plugin für Claude Code.

### MCP-Server Konfiguration

**Repository:** `publii-mcp` (separates Repository)

Die MCP-Server Konfiguration befindet sich im `publii-mcp` Repository, nicht in diesem Plugin. Der Server konfiguriert sich selbst über seine eigene `.mcp.json`:

```json
{
  "mcpServers": {
    "publii": {
      "command": "uv",
      "args": ["run", "publii-mcp", "serve"]
    }
  }
}
```

**Wichtig:** Beide Plugins müssen installiert sein:
```bash
claude plugins add /path/to/publii-mcp        # MCP-Server + Konfiguration
claude plugins add /path/to/publii-claude-code # Skills + Templates
```

### Skills

**Verzeichnis:** `skills/`

| Skill | Datei | Zweck |
|-------|-------|-------|
| post | `skills/post/SKILL.md` | Blog-Post Erstellung |
| page | `skills/page/SKILL.md` | Statische Seiten |

**Format:** YAML Front Matter + Markdown

```yaml
---
name: post
description: Erstellt Blog-Posts für Publii
---
# Skill-Inhalt als Markdown
```

### Templates

**Verzeichnis:** `skills/post/templates/`

| Template | Verwendung |
|----------|------------|
| wettkampfbericht.md | Turnier- und Wettkampfberichte |
| vereinsnews.md | Allgemeine Vereinsnachrichten |
| trainingsinfo.md | Trainingsankündigungen |

## Datenfluss

### Post-Erstellung

```
1. User: /publii:post "Wettkampfbericht"
          ↓
2. Skill: Lädt Template (wettkampfbericht.md)
          ↓
3. Skill: Fragt User nach Informationen
          ↓
4. Skill: Generiert HTML aus Template + Antworten
          ↓
5. MCP:   mcp__plugin_publii_publii__create_post(...)
          ↓
6. Server: Schreibt in Publii SQLite-Datenbank
          ↓
7. Skill: Bestätigt Erstellung (ID, Status)
```

### Site-Auswahl

```
1. Prüfe CLAUDE.md → publii_default_site
          ↓
   [Gefunden?] → Nutze diese Site
          ↓
   [Nicht gefunden?]
          ↓
2. MCP: list_sites → Zeige verfügbare Sites
          ↓
3. User wählt aus
```

## MCP-Tool Namensschema

Alle Tools folgen dem Muster:

```
mcp__plugin_publii_publii__<action>
│    │      │      │       │
│    │      │      │       └── Aktion (create_post, list_sites, ...)
│    │      │      └────────── MCP-Server Name (aus .mcp.json)
│    │      └───────────────── Plugin Name (aus plugin.json)
│    └──────────────────────── Präfix für Plugin-Tools
└───────────────────────────── MCP Namespace
```

**Beispiele:**
- `mcp__plugin_publii_publii__create_post`
- `mcp__plugin_publii_publii__list_sites`
- `mcp__plugin_publii_publii__update_page`

## Verzeichnisstruktur

### publii-claude-code (dieses Repository)

```
publii-claude-code/
├── .claude-plugin/
│   └── plugin.json          # Plugin-Metadaten
├── CLAUDE.md                 # Projekt-Konventionen
├── README.md                 # Benutzer-Dokumentation
├── LICENSE                   # MIT Lizenz
├── docs/
│   ├── API_REFERENCE.md      # MCP-Tool Dokumentation
│   ├── ARCHITECTURE.md       # Diese Datei
│   ├── TEMPLATE_AUTHORING.md # Template-Erstellung
│   └── TROUBLESHOOTING.md    # Problemlösungen
└── skills/
    ├── post/
    │   ├── SKILL.md          # Post-Skill Definition
    │   └── templates/
    │       ├── wettkampfbericht.md
    │       ├── vereinsnews.md
    │       └── trainingsinfo.md
    └── page/
        └── SKILL.md          # Page-Skill Definition
```

### publii-mcp (separates Repository)

```
publii-mcp/
├── .claude-plugin/
│   └── plugin.json          # Plugin-Metadaten
├── .mcp.json                 # MCP-Server Konfiguration
├── pyproject.toml           # Python-Projekt
└── src/
    └── publii_mcp/          # MCP-Server Implementierung
```

## Erweiterungspunkte

### Neuen Skill hinzufügen

1. Verzeichnis `skills/<name>/` erstellen
2. `SKILL.md` mit Front Matter anlegen
3. Optional: Templates unter `skills/<name>/templates/`

### Neues Template hinzufügen

1. Datei `skills/post/templates/<name>.md` erstellen
2. Struktur einhalten (siehe [Template Authoring](TEMPLATE_AUTHORING.md))
3. In `skills/post/SKILL.md` referenzieren

### Neuen MCP-Server einbinden

1. Separates MCP-Server Repository erstellen (mit eigener `.mcp.json`)
2. Tools in Skill-Dateien dokumentieren
3. API_REFERENCE.md aktualisieren
4. Beide Plugins installieren (`claude plugins add`)
