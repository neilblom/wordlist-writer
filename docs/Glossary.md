Glossary — WordList Writer
Version: 2026-07-17
Status: Authoritative Source of Truth

A
Accent Stripping
* Removing diacritical marks from characters (Greek, Latin) for normalized lookup.

API Route Shadowing
* When static middleware intercepts API requests, causing 404 or HTML fallback.

C
Cognate
* A word in another language sharing a historical root with an English word.
* Highlighted green (highest priority).
* Examples:
  * information ↔ informacion
  * manual ↔ manus
  * biology ↔ bios

COGNATE_MAP
* Unified lookup map containing { es, tier } for each English lemma.
* Built at startup from cognate JSON files and tier metadata.

Collector
* Logic inside renderHighlights() that pushes violation objects into curriculumViolations.

Curriculum Violation
* A mismatch between text and curriculum rules.
* Types:
  * Out-of-order vocabulary
  * Unknown word
  * Curriculum gap

Cognate Window
* Column B in the top panel.
* Displays cognates for the selected language.
* Clicking highlights matching tokens.

Cross-Language Master List
* Supabase table storing English, Spanish, Latin, Greek equivalents.
* Includes lemmas, cognate flags, frequency ranks, shared roots.

F
Frequency List
* Ranked list of common words (e.g., NGSL).
* Determines allowed (black) vs off-list (red) vocabulary.
* Stored as static JSON.

Frequency Rank
* Numeric indicator of word frequency.
* Lower rank = more common.

H
Hard Refresh
* Ctrl+Shift+R to bypass browser cache.

Highlighting Logic
* Strict priority:
  * Green = cognate
  * Black = known (frequency or project list)
  * Red = unknown

L
Lemma
* Canonical dictionary form of a word.
* Stored in Supabase as lemma.
* Examples:
  * running → run
  * went → go
  * mejores → mejor

Lemma Map
* JSON mapping inflected forms → lemma.
* One map per language.

M
Master List
* Curriculum sequence stored in Supabase.
* Tracks lemmas across all projects.
* Frontend uses array of word objects for UI rendering.

Master List Item (English-only)
* word
* language ("english")
* rank
* cognate (optional)
* cognates object (optional)

N
Normalization
* Convert text to NFD form and remove diacritics for consistent lookup.

P
Project
* Saved writing session containing text, word list, language, timestamp.
* Stored in Supabase.

project_id
* UUID linking Master List rows to a specific project.

Project Word List
* Custom list of words allowed for a specific project.
* Displayed in Column C.
* Words highlighted black.

R
Renderer
* Function (e.g., renderViolationsPanel) that converts data into UI output.

T
Tier
* Linguistic category for cognates:
  * Latin
  * Greek
  * Biblical
  * General
* Used for color-coded highlighting.

Tokenizer
* Splits text into tokens.
* Rules:
  * English/Spanish: whitespace + punctuation
  * Greek/Latin: Unicode-aware, accent-aware

Token
* Single unit of text produced by tokenizer.
* Example: "running," → "running"

V
Violations Panel
* Scrollable diagnostic UI showing curriculum violations in real time.

W
Writing Window
* Large bottom panel where text is typed.
* Words highlighted based on cognates, project list, frequency list.

Summary
This glossary defines core terminology used throughout WordList Writer. Update it whenever new concepts, lists, or features are introduced.

Documentation Formatting Reminder
All documentation updates must follow this formatting standard:
* Use plain text section titles
* Use asterisks (*) for bullet points
* Do not insert blank lines inside bullet lists
* Use ASCII-only characters
* Avoid Markdown headings (#)
* Avoid fenced code blocks unless necessary
* Use Step format for workflows to prevent GitHub auto-renumbering
This ensures consistent rendering across GitHub and prevents formatting breakage.
