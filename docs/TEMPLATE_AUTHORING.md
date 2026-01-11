# Template Authoring Guide

Anleitung zum Erstellen eigener Templates für den `/publii:post` Skill.

## Übersicht

Templates sind Markdown-Dateien, die dem Post-Skill eine Struktur vorgeben. Sie definieren:
- Welche Informationen vom User erfragt werden
- Wie das HTML-Gerüst aussieht
- Welche Fragen der Skill stellen soll

## Template-Speicherort

```
skills/post/templates/
├── wettkampfbericht.md
├── vereinsnews.md
├── trainingsinfo.md
└── [dein-template].md    ← Hier neue Templates anlegen
```

## Template-Struktur

Jedes Template **muss** folgende Abschnitte enthalten:

```markdown
# Template: [Name]

## Benötigte Informationen

[Liste der zu erfragenden Daten]

## HTML-Struktur

[HTML-Gerüst mit Platzhaltern]

## Beispiel-Fragen

[Formulierungsvorschläge]
```

### 1. Benötigte Informationen

Beschreibe, welche Daten der Skill vom User erfragen muss.

**Format:** Bullet-Liste mit kurzen Beschreibungen

```markdown
## Benötigte Informationen

Frage den User nach:
- Veranstaltungsname und Datum
- Ort der Veranstaltung
- Teilnehmer und ihre Ergebnisse
- Besondere Vorkommnisse oder Highlights
```

**Best Practices:**
- Sortiere nach Wichtigkeit (Pflichtfelder zuerst)
- Gruppiere zusammengehörige Informationen
- Halte die Liste überschaubar (4-8 Punkte)

### 2. HTML-Struktur

Das HTML-Gerüst mit Platzhaltern in eckigen Klammern.

**Format:** HTML-Codeblock mit Platzhaltern

```markdown
## HTML-Struktur

```html
<h2>Einleitung</h2>
<p>Am [Datum] fand [Veranstaltung] in [Ort] statt.</p>

<h2>Ergebnisse</h2>
<ul>
  <li><strong>[Name]</strong>: [Ergebnis]</li>
</ul>

<h2>Fazit</h2>
<p>[Zusammenfassung und Ausblick]</p>
```
```

**HTML-Regeln:**
- Kein `<h1>` (reserviert für Post-Titel)
- Beginne mit `<h2>` für Hauptabschnitte
- Nutze `<h3>` für Unterabschnitte
- Semantische Tags: `<strong>`, `<em>`, `<blockquote>`
- Keine Inline-Styles

**Platzhalter-Konventionen:**
- `[Name]` - Einfacher Wert
- `[Datum]` - Datum/Zeitangabe
- `[Beschreibung]` - Längerer Text
- `<!-- Weitere ... -->` - Wiederholbare Elemente

### 3. Beispiel-Fragen

Formulierungsvorschläge für den Skill, um die Informationen zu erfragen.

**Format:** Nummerierte Liste mit Fragen in Anführungszeichen

```markdown
## Beispiel-Fragen

1. "Wie hieß die Veranstaltung und wann fand sie statt?"
2. "Wo wurde sie ausgetragen?"
3. "Wer hat teilgenommen und wie waren die Ergebnisse?"
4. "Gab es besondere Highlights?"
```

**Best Practices:**
- Eine Frage pro Informationsgruppe
- Natürliche, gesprächsnahe Formulierung
- Reihenfolge wie in "Benötigte Informationen"

## Vollständiges Beispiel

```markdown
# Template: Mitgliederversammlung

## Benötigte Informationen

Frage den User nach:
- Datum und Uhrzeit der Versammlung
- Ort (Vereinsheim, online, etc.)
- Wichtige Tagesordnungspunkte
- Beschlüsse und Abstimmungsergebnisse
- Nächste Termine

## HTML-Struktur

```html
<h2>Einladung zur Mitgliederversammlung</h2>
<p>
  Hiermit laden wir herzlich zur ordentlichen Mitgliederversammlung ein.
</p>

<h3>Termin</h3>
<p>
  <strong>Datum:</strong> [Datum]<br>
  <strong>Uhrzeit:</strong> [Uhrzeit]<br>
  <strong>Ort:</strong> [Ort]
</p>

<h3>Tagesordnung</h3>
<ol>
  <li>Begrüßung und Feststellung der Beschlussfähigkeit</li>
  <li>[Tagesordnungspunkt]</li>
  <li>Verschiedenes</li>
</ol>

<h3>Beschlüsse</h3>
<ul>
  <li>[Beschluss]: [Ergebnis der Abstimmung]</li>
</ul>

<p>
  Wir bitten um zahlreiches Erscheinen!<br>
  <em>Der Vorstand</em>
</p>
```

## Beispiel-Fragen

1. "Wann findet die Versammlung statt (Datum und Uhrzeit)?"
2. "Wo wird sie abgehalten?"
3. "Welche Punkte stehen auf der Tagesordnung?"
4. "Wurden bereits Beschlüsse gefasst? Wenn ja, welche?"
```

## Template im Skill verwenden

Nach dem Erstellen kann das Template so aufgerufen werden:

```bash
/publii:post --template mitgliederversammlung
```

Der Skill:
1. Lädt `skills/post/templates/mitgliederversammlung.md`
2. Zeigt "Benötigte Informationen" an
3. Stellt "Beispiel-Fragen"
4. Füllt "HTML-Struktur" mit Antworten
5. Erstellt den Post

## Tipps für gute Templates

### Wiederverwendbarkeit

Templates sollten für verschiedene Instanzen des gleichen Post-Typs funktionieren:

**Gut:** "Wettkampfbericht" (funktioniert für jeden Wettkampf)
**Schlecht:** "Bericht Stadtmeisterschaft 2024" (zu spezifisch)

### Vollständigkeit

Decke alle typischen Inhalte ab, aber halte es flexibel:

```html
<h2>Highlights</h2>
<p>[Besondere Momente, Erfolge, oder "keine besonderen Vorkommnisse"]</p>
```

### Konsistenz

Halte dich an die bestehenden Templates:
- Gleiche Abschnittsreihenfolge
- Ähnliche Überschriften-Hierarchie
- Vergleichbare Detailtiefe

## Checkliste für neue Templates

- [ ] Datei unter `skills/post/templates/[name].md` erstellt
- [ ] Alle drei Pflichtabschnitte vorhanden
- [ ] HTML beginnt mit `<h2>`, nicht `<h1>`
- [ ] Platzhalter in eckigen Klammern `[...]`
- [ ] Deutsche Umlaute korrekt (UTF-8)
- [ ] Beispiel-Fragen natürlich formuliert
- [ ] Template in SKILL.md Tabelle eingetragen (optional)
