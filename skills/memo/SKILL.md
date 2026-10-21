---
name: memo
version: 6.0.0
description: >
  Memo-Stil für Projekte in AIOS-Vaults: legt Memos und Specs im @Memo/- bzw. @Spec/-Ordner
  des Projekts an — Nummernvergabe, Dateiname, Header-Block bzw. Spec-Frontmatter,
  Struktur, Formatierung, Obsidian-LaTeX; Memos sind ein Notizbuch und werden nie geändert.
  Verwenden, wenn ein Memo verlangt wird ("schreib ein Memo", "halte das fest",
  "Folge-Memo", "Spec schreiben") oder ein anderer Skill ein Memo erzeugt.
---

# Memo-Stil

**Datum:** 2026-10-01
**Status:** Konvention (self-demonstrating)

## Zweck

Dieser Skill beschreibt den Stil, in dem Memos in den `@Memo/`-Ordnern eines Vaults verfasst werden. Er ist zugleich **Beispiel** für den Stil, den er beschreibt: Wer diese Datei ansieht, sieht das Muster.

## Geltungsbereich

Die Konvention gilt für **Memos** (`@Memo/`) und **Specs** (`@Spec/`). Specs unterscheiden sich nur im Kopf: Sie tragen ein YAML-Frontmatter statt des Header-Blocks (siehe *Spec-Frontmatter*). Die Konvention gilt **nicht** für Regeln und Skills in Masterspaces (`@/@Rule/`, `@/@!Skill/<name>/SKILL.md`). Diese tragen ein YAML-Frontmatter (`name`, `description`), damit sie als Skill ausgelöst werden können — wie diese Datei selbst.

## Grundregel

Memos sind **Obsidian-kompatibles Markdown**. Sie müssen sich in einem Obsidian-Vault korrekt rendern, weil die `@Memo/`-Ordner Teil des Vaults sind.

## Memos und Specs — zwei Arten von Dokumenten

| | Memos (`@Memo/`) | Specs (`@Spec/`) |
|---|---|---|
| Charakter | Notizbuch: historische Folge, das Verständnis wächst von Memo zu Memo | Regelwerk: beschreibt den gültigen Soll-Stand |
| Frühere Dokumente | werden **nie geändert** | werden **aktualisiert**, wenn sich der Soll-Stand ändert |
| Korrekturen, Verfeinerungen | in ein **neues Memo** mit der nächsten Nummer; es darf frühere Memos inhaltlich ersetzen | in der betroffenen Spec selbst |
| Wiederholung, Widerspruch | erlaubt: ein späteres Memo darf ein früheres wiederholen, präzisieren oder ihm widersprechen | verboten: mehrere Specs in einem Ordner dürfen sich weder wiederholen noch widersprechen |
| Gültigkeit | bei Widerspruch gilt das spätere Memo | alle Specs gelten zugleich |

**Memos:** Ein bestehendes Memo wird nicht geändert, auch nicht, um auf ein späteres zu verweisen oder einen Begriff nachzuziehen. Am Anfang dürfen grobe, teilweise ungenaue Notizen stehen, die in weiteren Memos an Qualität gewinnen, so wie in einem Notizbuch das Verständnis wächst. Ersetzt ein neues Memo ein früheres ganz oder teilweise, sagt es das im Header mit `**Ersetzt:**` (z. B. `Memo 05, Abschnitt Projektstatus`). Änderungen an bestehenden Memos nur auf ausdrücklichen Wunsch des Users.

**Specs:** Überschneiden sich zwei Specs, verweist die eine auf die andere, statt den Inhalt zu wiederholen. Nach jeder Änderung einer Spec werden die übrigen Specs desselben Ordners auf Wiederholungen und Widersprüche geprüft.

## Anwendung — ein Memo anlegen

Wird ein Memo verlangt („schreib ein Memo", „halte das fest", „Folge-Memo") oder erzeugt ein anderer Skill ein Memo als Ergebnis, gilt dieser Ablauf:

1. **Ordner bestimmen:** der `@Memo/`-Ordner des aktuellen Projekts (Arbeitsverzeichnis oder vom User genannter Projektordner). Für Spezifikationen gilt analog `@Spec/`. Dabei gilt in dieser Reihenfolge:

   | # | Zustand | Verhalten |
   |---|---|---|
   | 1 | `<projekt>/@Memo/` existiert | Memo mit der nächsten Nummer anlegen |
   | 2 | `@Memo/` fehlt, aber `<projekt>/@/` existiert | `@Memo/` ohne Rückfrage anlegen, dann Memo `01 …` |
   | 3 | weder `@Memo/` noch `@/` existiert | **kein** Memo anlegen, sondern nachfragen (Dialog unten) |

   Das `@`-Verzeichnis kennzeichnet einen Ordner als Projekt. Nur dort legt der Skill eigenständig Struktur an. **Dialog im Fall 3:** mitteilen, dass der Ordner kein Projekt ist, und vorschlagen, ihn mit `/aios-init` zu initialisieren („Der Ordner `<projekt>` ist kein Projekt (kein `@`-Ordner). Deshalb lege ich kein Memo an. Soll ich ihn mit `/aios-init` als Projekt initialisieren?"). Bei Nein abbrechen, nichts anlegen. Bei Ja `/aios-init` ausführen und **als eigenen Schritt** fragen, ob jetzt das Memo erstellt werden soll; erst bei erneutem Ja greift Fall 1 oder 2. Die beiden Zustimmungen werden getrennt eingeholt.
2. **Nummer vergeben:** die höchste vorhandene Nummer im Ordner plus 1, mindestens zweistellig mit führender Null (`01`, `02`, …), ab 100 dreistellig. Es zählen alle Dateien im Ordner, mit oder ohne `!!!`. Nummern werden nie neu vergeben oder umsortiert, Lücken bleiben. `@Memo/` und `@Spec/` haben je eine eigene Nummernfolge.
3. **Dateiname bilden:** `NN Sprechender Titel.md`, mit `!!!` nur bei den unten genannten Kriterien.
4. **Schreiben:** direkt in den Ordner, im hier beschriebenen Format. Bestehende Memos werden dabei nicht geändert (siehe *Memos und Specs*).
5. **Versionieren:** Liegt der Ordner in einem Git-Repo, das Memo committen, sofern der User nichts anderes sagt.
6. **Melden:** dem User den Pfad als Link nennen.

## Datei-Konvention

| Aspekt | Vorgabe |
|---|---|
| Ablage | `@Memo/`-Unterordner des Projekts (Spezifikationen: `@Spec/`) |
| Dateiname | `NN Sprechender Titel.md`, fortlaufend nummeriert |
| Encoding | UTF-8, mit Sonderzeichen (Umlaute, `§` usw.) |
| Frontmatter | Memos: **keines**, der Header-Block übernimmt die Rolle. Specs: YAML-Frontmatter **statt** Header-Block |
| Hervorhebung | optional `NN !!! Sprechender Titel.md`, drei Ausrufezeichen direkt nach der Nummer markieren ein besonders wichtiges Memo (siehe *Hervorhebungs-Konvention*) |

## Hervorhebungs-Konvention — `!!!` im Dateinamen

Memos mit besonderer konzeptioneller Bedeutung können durch drei Ausrufezeichen nach der Nummerierung hervorgehoben werden:

```
NN !!! Sprechender Titel.md
```

Beispiel: `@Memo/01 !!! Grundbegriffe des Projekts.md`

**Wirkung:**

- **Sortier-Erhalt:** Die Nummerierung bleibt vorn, die Reihenfolge im Verzeichnis ist unverändert.
- **Visueller Anker:** `!!!` springt beim Auflisten ins Auge, der Leser erkennt sofort die priorisierten Dokumente.
- **Tool-freundlich:** keine Sonderdatei, kein Frontmatter-Tag, kein versteckter Mechanismus. Die Hervorhebung ist im Dateinamen sichtbar und filterbar (`ls | grep "!!!"`).

**Wann verwenden?** Nur sparsam und bewusst: für Memos, die als zentrale konzeptuelle Anker dienen, einen wichtigen Perspektivwechsel dokumentieren oder als Einstiegspunkt für künftige Sessions empfohlen werden. Nicht für Routine-Notizen, Entscheidungs-Memos zu Alltagsfragen oder kurze Erklärungen.

## Header-Block

Direkt unter dem `# Titel` (H1) stehen die Metadaten-Zeilen in `**Feld:**`-Notation, ohne abschließendes `---`:

```markdown
# Titel des Memos

**Datum:** YYYY-MM-DD
**Status:** Empfehlung | Entscheidung | Konvention | Notiz | Spec | …

## Erste Sektion
```

| Feld | Pflicht | Inhalt |
|---|---|---|
| `**Datum:**` | ja | Erstellungsdatum, `YYYY-MM-DD` |
| `**Status:**` | ja | Art bzw. Reifegrad des Memos |
| `**Betrifft:**` | optional | Gegenstand, betroffene Dateien oder Systeme |
| `**Autor:**` | optional | Kürzel oder Name |
| `**Quelle:**` | optional | Herkunft des Inhalts |
| `**Verwandt:**` | optional | Kurzverweis, wenn keine eigene `Verwandt`-Sektion nötig ist |
| `**Ersetzt:**` | optional | frühere Memos oder Abschnitte, die dieses Memo inhaltlich ersetzt, z. B. `Memo 05, Abschnitt Projektstatus` |

Die Reihenfolge ist Datum, Status, danach die optionalen Felder. Andere Felder werden in Memos nicht verwendet.

## Spec-Frontmatter

Specs in `@Spec/` beginnen mit einem YAML-Frontmatter. Es ersetzt den Header-Block; danach folgt direkt der `# Titel`.

```yaml
---
title: "Beispiel"
date: 2026-10-01
author: Kürzel
status: Spec
tags: [spec, beispiel]
number: 01
related:
  - "@Memo/03 Entscheidung zum Beispiel.md"
---
```

| Feld | Pflicht | Inhalt |
|---|---|---|
| `title` | ja | Titel, identisch mit dem `# Titel` |
| `date` | ja | Datum des Stands, also der letzten inhaltlichen Änderung, `YYYY-MM-DD` |
| `status` | ja | z. B. `Spec`, `Blueprint`, `Entwurf` |
| `number` | ja | Nummer `NN` aus dem Dateinamen |
| `author` | optional | Kürzel oder Name |
| `tags` | optional | Liste, erster Eintrag `spec` |
| `related` | optional | Liste verwandter Pfade (Skills, Memos) |

## Struktur

- `# Titel`: **genau einmal**, als erste Zeile
- `## Sektion`: Hauptabschnitte mit sprechenden Substantiven (`Problem`, `Lösung`, `Verifikation`, `Caveats`, `Verwandt`)
- `### Unter-Abschnitt`: sparsam, nur wenn nötig
- Trennlinien `---` zwischen Sektionen **nicht** verwenden
- Em-Dash `—` in Sektionstiteln, wenn man präzisieren will: `## Lösung — Symlink in $HOME`

## Formatierung

- **Fließtext bevorzugen**, Bullet-Listen nur, wenn die Aufzählung explizit ungeordnet ist
- **Tabellen** für Vergleiche und Eigenschafts-Listen (Markdown-Standard mit `|`)
- **Code-Blöcke** mit Sprach-Tag (` ```bash`, ` ```yaml`, ` ```markdown`)
- **`Inline-Code`** für Pfade, Befehle, Variablennamen und Dateinamen
- **Fett** für Lead-Ins in Bullet-Listen: `- **Aspekt:** Erklärung`
- Verschachtelung von Bullets so flach wie möglich (höchstens eine Ebene)

## LaTeX (Obsidian-kompatibel)

Obsidian rendert LaTeX via **MathJax**. Folgende Syntax verwenden:

| Verwendung | Syntax | Beispiel | Rendert als |
|---|---|---|---|
| Inline-Formel | `$...$` | `$F = m \cdot a$` | $F = m \cdot a$ |
| Block-Formel | `$$...$$` | `$$\int_0^1 x^2 \, dx$$` | $$\int_0^1 x^2 \, dx$$ |
| Dezimalkomma | `{,}` statt `,` | `$3{,}14$` | $3{,}14$ |
| Einheiten | `\,\text{...}` | `$1{,}7\,\text{mm}$` | $1{,}7\,\text{mm}$ |
| Pfeile | `\rightarrow` | `$a \rightarrow b$` | $a \rightarrow b$ |
| Toleranz | `\pm` | `$\pm 0{,}02\,\text{mm}$` | $\pm 0{,}02\,\text{mm}$ |
| Durchmesser | `\varnothing` | `$\varnothing\, 0{,}1\,\text{mm}$` | $\varnothing\, 0{,}1\,\text{mm}$ |

**Wichtig — Obsidian-Einschränkungen:**

- **Nur** `$...$` und `$$...$$` verwenden. `\(...\)` und `\[...\]` rendert Obsidian **nicht**.
- **Kein einzelnes `$`** in normalem Text ohne LaTeX, weil das den Math-Modus auslöst. Falls nötig, mit `\$` escapen.
- **Leerzeichen** vor `$` und nach dem schließenden `$` lassen, sonst wird die Formel oft nicht erkannt.
- Das Dezimalkomma `{,}` ist im deutschen Kontext wichtig. Ohne die geschweiften Klammern fügt LaTeX einen Wortzwischenraum ein.

## Sektion-Muster (Empfehlung)

Typische Abfolge für **Erklärungs- und Lösungs-Memos**:

1. **Zweck** oder **Problem**: was zur Debatte steht
2. **Lösung** oder **Empfehlung**: was vorgeschlagen wird
3. **Umsetzung** oder **Verifikation**: wie und dass es funktioniert
4. **Eigenschaften** oder **Vergleich**: Tabelle mit Aspekten
5. **Caveats** oder **Offen**: was zu beachten ist
6. **Verwandt**: Querverweise zu anderen Memos

Für **Entscheidungs-Memos**:

1. **Anforderung**: was gefordert war
2. **Optionen**: Tabelle mit Alternativen
3. **Empfehlung** + Begründung: gewählte Option
4. **Offen**: Punkte, die noch zu klären sind
