# fixture-aat — skill.md

## Version

```text
fixture-aat v1
```

---

# Purpose

Extract, filter, curate and normalize tennis interclub fixture data from an AAT / deportivoaat URL.

The skill receives:

* an AAT URL
* optional category preferences
* optional date overrides
* optional normalization mappings

and returns:

* structured fixture data
* a human review layer
* confirmation flow before downstream usage

This skill is focused exclusively on fixture extraction and curation.

---

# Scope

This skill is intended to feed downstream systems such as:

* story generators
* sports graphics
* editorial layouts
* automation pipelines
* overlays
* social media assets

The skill acts as a fixture data pipeline.

---

# Inputs

## Optional

### `url`

AAT / deportivoaat URL to analyze.

If the user does not provide a URL, the skill must automatically use the default URL:

    https://deportivoaat.com.ar/sis/app/vEntidad.php?idEntidad=214

The skill must not ask the user for another URL if this default URL is available.

Example:

    {
      "url": "https://deportivoaat.com.ar/sis/app/vEntidad.php?idEntidad=214"
    }

---

# Club Configuration

## `club_name`

Canonical club name used for LOCAL / VISITANTE detection.

Default:

```text
SOLANA TENIS CLUB A.CIVIL
```

---

## `club_aliases`

Alternative readable names for the same club.

Default:

```json
[
  "Complejo Solana",
  "SOLANA",
  "Solana Tenis Club"
]
```

All aliases must be treated as the same club.

---

# URL Resolution Rules

When resolving which AAT URL to use, follow this priority order:

1. User-provided AAT / deportivoaat URL
2. Default AAT entity URL:

       https://deportivoaat.com.ar/sis/app/vEntidad.php?idEntidad=214

If either one exists, continue the extraction flow normally.

Do not stop execution to ask for another URL unless:
- the provided/default URL cannot be accessed, or
- the page cannot be parsed, or
- the extraction fails completely.

---

# Default Date Selection

By default, the skill analyzes the nearest upcoming:

* Saturday
* Sunday

based on the current date when the request is made.

Before finalizing the fixture list, the skill must ask the user whether there is any adjacent holiday before or after the weekend.

If confirmed, the holiday dates are included in the analyzed range.

---

# Optional Inputs

## `date_range`

Explicit dates to analyze.

Example:

```json
{
  "start_date": "2026-05-23",
  "end_date": "2026-05-25"
}
```

---

## `categories`

Custom categories to include.

Example:

```json
{
  "categories": [
    "CABALLEROS +19",
    "DAMAS +30",
    "MIXTO LIBRE"
  ]
}
```

---

## `club_name_map`

Explicit club normalization mappings.

Example:

```json
{
  "club_name_map": {
    "ASOC. DEP. RACIONALES": "Deportes Racionales",
    "GAZCON LAWN TENNIS CLUB": "Gazcon LTC"
  }
}
```

User-defined mappings always override automatic normalization.

---

# Default Categories

By default, the skill includes:

## Caballeros — Interclubes por Edad

* +19
* +25
* +30

## Caballeros — Libres

* Tercera
* Cuarta
* Quinta

Unless explicitly requested, the skill ignores:

* Damas
* Mixto
* +50
* juniors
* unrelated categories

---

# Friendly Reminder

The skill should gently remind the user that additional categories can also be included if desired.

Example:

```text
Currently filtering:
- Caballeros +19 / +25 / +30
- Libres 3ra / 4ta / 5ta

You can also request:
- Damas
- Mixto
- +50
- other divisions
- specific categories
```

---

# Valid Fixture Rule

A row is considered a valid fixture only if it contains:

* category
* date
* time
* home club
* away club

If any of these fields are missing, the skill must not invent or infer the missing value.

Incomplete rows must be placed in a review section or ignored until the user confirms them.

---

# Fixture Extraction Rules

The skill must extract only real fixture rows from the AAT / deportivoaat page.

The AAT / deportivoaat page is the source of truth.

Do not invent:

* opponents
* dates
* times
* divisions
* local / visitor condition

If the page is ambiguous, return the ambiguity clearly and ask for confirmation.

---

# Team Variant Handling

The skill must not treat team variants as duplicated fixtures.

Examples:

* SOLANA A
* SOLANA B
* SOLANA C

These represent distinct competitive entities.

---

# Duplicate Detection Rule

Duplicate detection must consider:

* category
* division
* date
* time
* home club
* away club
* team variant

before deciding that two fixtures are duplicates.

---

# Local / Visitor Rule

If the target club appears first:

```text
SOLANA vs Club X
```

→ `LOCAL`

If the target club appears second:

```text
Club X vs SOLANA
```

→ `VISITANTE`

---

# Club Name Normalization

The skill must normalize club names for readability while preserving identity.

The goal is not to reproduce the raw federation name exactly, but to produce a clean name suitable for fixture cards and Instagram stories.

---

# General Cleanup Rules

Remove legal or administrative suffixes such as:

```text
A.CIVIL
ASOCIACIÓN CIVIL
ASOC. CIVIL
ASOC.
```

Normalize excessive uppercase into title case, except known acronyms.

---

# Lawn Tennis Club Rule

Any club containing:

```text
LAWN TENNIS CLUB
```

may be shortened to:

```text
LTC
```

Example:

```text
GAZCON LAWN TENNIS CLUB
→ Gazcon LTC
```

---

# Tennis Club Abbreviation Rule

Preserve `Tenis Club` when the resulting name remains visually clean and reasonably short.

Examples:

```text
El Prado Tenis Club
Oasis Tenis Club
```

If the name becomes excessively long, the skill may abbreviate:

```text
Tenis Club → TC
```

Readability and visual cleanliness should always be prioritized.

---

# Display Name Formatting

The skill should generate compact display names optimized for visual layouts and social media readability.

Preferred format:

```text
+19 - 4ta B
+30 - Segunda
+25 - Intermedia
Libres - Cuarta
```

Avoid verbose federation naming.

---

# Fixture Card Content

Each fixture should expose:

* `category_display`
* `time`
* `match_display`
* `condition`

Example:

```json
{
  "category_display": "Libres - Tercera",
  "time": "14:00",
  "match_display": "SOLANA vs Temperley LTC",
  "condition": "LOCAL"
}
```

---

# Structured Output

The skill must produce machine-friendly structured output grouped by day.

Example:

```json
{
  "days": [
    {
      "date": "2026-05-24",
      "display_date": "DOMINGO 24/05",
      "fixtures": [
        {
          "category_display": "+19 - 4ta B",
          "time": "14:15",
          "match_display": "SOLANA vs Deportes Racionales",
          "condition": "LOCAL"
        }
      ]
    }
  ]
}
```

---

# Human Review Output

Before any downstream usage, the skill must show a human-readable review layer.

Example:

```text
SERIES ENCONTRADAS — FIN DE SEMANA

DOMINGO 24/05

• +19 - 4ta B
• 14:15 hs
• SOLANA vs Deportes Racionales
• LOCAL
```

---

# Incomplete or Ambiguous Fixtures

If a fixture is incomplete or ambiguous, the skill must explicitly request review.

Example:

```text
Some fixtures could not be fully validated and need review:

• +19 - 4ta B
• Missing opponent
• Sunday 24/05

Can you confirm the missing information?
```

The skill must prioritize:

* accuracy
* transparency
* verifiable extraction

over trying to force a complete output.

---

# Final Confirmation Flow

Before completing the extraction pipeline, the skill must:

1. show structured output
2. show human-readable review output
3. ask for explicit confirmation

Example:

```text
¿Confirmás que use estas series?
```

Even if extraction appears correct, confirmation is still required before downstream usage.

---

# Non Goals

This skill does NOT:

* generate images
* generate stories
* generate graphics
* apply branding
* choose typography
* create layouts
* perform visual rendering
* invent fixture information
* assume missing values
* silently ignore ambiguities

Visual generation should be handled by separate downstream skills.

---

# Usage Examples

## Default Usage

```json
{
  "url": "https://deportivoaat.com.ar/sis/app/vEntidad.php?idEntidad=214"
}
```

---

## Custom Categories

```json
{
  "url": "https://deportivoaat.com.ar/sis/app/vEntidad.php?idEntidad=214",
  "categories": [
    "DAMAS +30",
    "MIXTO LIBRE"
  ]
}
```

---

## Explicit Date Range

```json
{
  "url": "https://deportivoaat.com.ar/sis/app/vEntidad.php?idEntidad=214",
  "date_range": {
    "start_date": "2026-05-23",
    "end_date": "2026-05-25"
  }
}
```

---

## Custom Club Mapping

```json
{
  "url": "https://deportivoaat.com.ar/sis/app/vEntidad.php?idEntidad=214",
  "club_name_map": {
    "ASOC. DEP. RACIONALES": "Deportes Racionales",
    "GAZCON LAWN TENNIS CLUB": "Gazcon LTC"
  }
}
```
