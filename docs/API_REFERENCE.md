# API Reference

Vollständige Referenz aller MCP-Tools des Publii Claude Code Plugins.

## Übersicht

Alle Tools folgen dem Namensschema: `mcp__plugin_publii_publii__<action>`

| Tool | Beschreibung |
|------|--------------|
| `list_sites` | Alle verfügbaren Sites auflisten |
| `list_posts` | Posts einer Site auflisten |
| `get_post` | Einzelnen Post abrufen |
| `create_post` | Neuen Post erstellen |
| `update_post` | Bestehenden Post aktualisieren |
| `list_pages` | Seiten einer Site auflisten |
| `get_page` | Einzelne Seite abrufen |
| `create_page` | Neue Seite erstellen |
| `update_page` | Bestehende Seite aktualisieren |
| `delete_page` | Seite löschen |
| `list_tags` | Tags einer Site auflisten |
| `list_authors` | Autoren einer Site auflisten |

---

## Sites

### list_sites

Listet alle verfügbaren Publii Sites auf.

**Parameter:** Keine

**Rückgabe:**
```json
{
  "sites": [
    {
      "name": "Mein Blog",
      "slug": "mein-blog",
      "path": "/Users/user/Documents/Publii/sites/mein-blog"
    }
  ]
}
```

**Beispiel:**
```
Nutze mcp__plugin_publii_publii__list_sites
```

---

## Posts

### list_posts

Listet alle Posts einer Site auf.

**Parameter:**

| Parameter | Typ | Erforderlich | Beschreibung |
|-----------|-----|--------------|--------------|
| `site` | string | Ja | Site-Slug |

**Rückgabe:**
```json
{
  "posts": [
    {
      "id": 1,
      "title": "Mein erster Post",
      "slug": "mein-erster-post",
      "status": "published",
      "created_at": "2024-01-15T10:30:00",
      "author_id": 1
    }
  ]
}
```

### get_post

Ruft einen einzelnen Post mit vollständigem Inhalt ab.

**Parameter:**

| Parameter | Typ | Erforderlich | Beschreibung |
|-----------|-----|--------------|--------------|
| `site` | string | Ja | Site-Slug |
| `post_id` | integer | Ja | Post-ID |

**Rückgabe:**
```json
{
  "id": 1,
  "title": "Mein erster Post",
  "slug": "mein-erster-post",
  "content": "<p>Inhalt des Posts...</p>",
  "status": "published",
  "created_at": "2024-01-15T10:30:00",
  "modified_at": "2024-01-15T14:20:00",
  "author_id": 1,
  "tags": ["news", "update"]
}
```

### create_post

Erstellt einen neuen Blog-Post.

**Parameter:**

| Parameter | Typ | Erforderlich | Beschreibung |
|-----------|-----|--------------|--------------|
| `site` | string | Ja | Site-Slug |
| `title` | string | Ja | Post-Titel |
| `content` | string | Ja | HTML-Inhalt |
| `author_id` | integer | Nein | Autor-ID (Standard: 1) |
| `status` | string | Nein | `draft` oder `published` (Standard: `draft`) |
| `tags` | array | Nein | Liste von Tag-Namen |
| `slug` | string | Nein | URL-Slug (wird aus Titel generiert wenn leer) |

**Rückgabe:**
```json
{
  "success": true,
  "post_id": 42,
  "message": "Post erfolgreich erstellt"
}
```

**Beispiel:**
```json
{
  "site": "mein-blog",
  "title": "Wettkampfbericht Leipzig",
  "content": "<h2>Ergebnisse</h2><p>...</p>",
  "author_id": 1,
  "status": "draft",
  "tags": ["wettkampf", "2024"]
}
```

### update_post

Aktualisiert einen bestehenden Post.

**Parameter:**

| Parameter | Typ | Erforderlich | Beschreibung |
|-----------|-----|--------------|--------------|
| `site` | string | Ja | Site-Slug |
| `post_id` | integer | Ja | Post-ID |
| `title` | string | Nein | Neuer Titel |
| `content` | string | Nein | Neuer HTML-Inhalt |
| `status` | string | Nein | Neuer Status |
| `tags` | array | Nein | Neue Tag-Liste |

**Rückgabe:**
```json
{
  "success": true,
  "message": "Post erfolgreich aktualisiert"
}
```

---

## Pages

### list_pages

Listet alle statischen Seiten einer Site auf.

**Parameter:**

| Parameter | Typ | Erforderlich | Beschreibung |
|-----------|-----|--------------|--------------|
| `site` | string | Ja | Site-Slug |

**Rückgabe:**
```json
{
  "pages": [
    {
      "id": 1,
      "title": "Impressum",
      "slug": "impressum",
      "status": "published"
    }
  ]
}
```

### get_page

Ruft eine einzelne Seite mit vollständigem Inhalt ab.

**Parameter:**

| Parameter | Typ | Erforderlich | Beschreibung |
|-----------|-----|--------------|--------------|
| `site` | string | Ja | Site-Slug |
| `page_id` | integer | Ja | Seiten-ID |

**Rückgabe:**
```json
{
  "id": 1,
  "title": "Impressum",
  "slug": "impressum",
  "content": "<h2>Angaben gemäß § 5 TMG</h2><p>...</p>",
  "status": "published"
}
```

### create_page

Erstellt eine neue statische Seite.

**Parameter:**

| Parameter | Typ | Erforderlich | Beschreibung |
|-----------|-----|--------------|--------------|
| `site` | string | Ja | Site-Slug |
| `title` | string | Ja | Seiten-Titel |
| `content` | string | Ja | HTML-Inhalt |
| `status` | string | Nein | `draft` oder `published` (Standard: `draft`) |
| `slug` | string | Nein | URL-Slug (wird aus Titel generiert wenn leer) |

**Rückgabe:**
```json
{
  "success": true,
  "page_id": 5,
  "message": "Seite erfolgreich erstellt"
}
```

### update_page

Aktualisiert eine bestehende Seite.

**Parameter:**

| Parameter | Typ | Erforderlich | Beschreibung |
|-----------|-----|--------------|--------------|
| `site` | string | Ja | Site-Slug |
| `page_id` | integer | Ja | Seiten-ID |
| `title` | string | Nein | Neuer Titel |
| `content` | string | Nein | Neuer HTML-Inhalt |
| `status` | string | Nein | Neuer Status |

**Rückgabe:**
```json
{
  "success": true,
  "message": "Seite erfolgreich aktualisiert"
}
```

### delete_page

Löscht eine Seite.

**Parameter:**

| Parameter | Typ | Erforderlich | Beschreibung |
|-----------|-----|--------------|--------------|
| `site` | string | Ja | Site-Slug |
| `page_id` | integer | Ja | Seiten-ID |

**Rückgabe:**
```json
{
  "success": true,
  "message": "Seite erfolgreich gelöscht"
}
```

**Hinweis:** Diese Aktion kann nicht rückgängig gemacht werden.

---

## Tags & Authors

### list_tags

Listet alle Tags einer Site auf.

**Parameter:**

| Parameter | Typ | Erforderlich | Beschreibung |
|-----------|-----|--------------|--------------|
| `site` | string | Ja | Site-Slug |

**Rückgabe:**
```json
{
  "tags": [
    {
      "id": 1,
      "name": "News",
      "slug": "news",
      "post_count": 15
    }
  ]
}
```

### list_authors

Listet alle Autoren einer Site auf.

**Parameter:**

| Parameter | Typ | Erforderlich | Beschreibung |
|-----------|-----|--------------|--------------|
| `site` | string | Ja | Site-Slug |

**Rückgabe:**
```json
{
  "authors": [
    {
      "id": 1,
      "name": "Admin",
      "email": "admin@example.com",
      "post_count": 42
    }
  ]
}
```

---

## Fehlerbehandlung

Alle Tools können folgende Fehler zurückgeben:

| Fehlercode | Beschreibung |
|------------|--------------|
| `SITE_NOT_FOUND` | Site existiert nicht |
| `POST_NOT_FOUND` | Post mit dieser ID existiert nicht |
| `PAGE_NOT_FOUND` | Seite mit dieser ID existiert nicht |
| `AUTHOR_NOT_FOUND` | Autor mit dieser ID existiert nicht |
| `INVALID_PARAMETER` | Ungültiger Parameter-Wert |
| `PERMISSION_DENIED` | Keine Berechtigung für diese Aktion |
| `DATABASE_ERROR` | Fehler beim Datenbankzugriff |

**Fehler-Rückgabe Format:**
```json
{
  "success": false,
  "error": "SITE_NOT_FOUND",
  "message": "Site 'nicht-existierend' wurde nicht gefunden"
}
```

---

## HTML-Richtlinien

### Erlaubte Tags

```html
<h2>, <h3>, <h4>    <!-- Überschriften (kein h1!) -->
<p>                  <!-- Absätze -->
<ul>, <ol>, <li>     <!-- Listen -->
<strong>, <em>       <!-- Hervorhebungen -->
<a href="...">       <!-- Links -->
<blockquote>         <!-- Zitate -->
<code>, <pre>        <!-- Code -->
<img src="...">      <!-- Bilder -->
<table>, <tr>, <td>  <!-- Tabellen -->
```

### Best Practices

1. **Kein `<h1>`** - wird für den Post-Titel reserviert
2. **Semantisches HTML** - `<strong>` statt `<b>`, `<em>` statt `<i>`
3. **Keine Inline-Styles** - CSS wird vom Theme gesteuert
4. **Alt-Texte für Bilder** - `<img src="..." alt="Beschreibung">`
