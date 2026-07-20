Glossary — WordList Writer
Version: 2026-07-20
Status: Authoritative Source of Truth

A
Accent Stripping
* removing diacritical marks from characters for normalized lookup
* used in normalization pipeline for Spanish, Greek, and Latin

API Route Shadowing
* when Express static middleware intercepts API requests
* causes 404 or HTML fallback
* fixed by registering API routes before express.static()

C
Cognate
* a word in another language sharing a historical root with an English word
* highlighted green (highest priority)
* examples:
  * information ↔ informacion
  * manual ↔ manus
  * biology ↔ bios

COGNATE_MAP
* unified lookup map containing tier metadata for each lemma
* built at startup from cognate JSON files
* consumed by highlight pipeline and project list

Collector
* logic inside highlight pipeline that pushes violation objects into curriculumViolations

Curriculum Violation
* mismatch between text and curriculum rules
* types:
  * out-of-order vocabulary
  * unknown word
  * curriculum gap

Cognate Window
* Column B in the top panel
* displays cognates for the selected language
* clicking highlights matching tokens and inserts cognate into master list

Cross-Language Master List
* Supabase table storing English, Spanish, Latin, and Greek equivalents
* includes lemmas, cognate flags, frequency ranks, shared roots

F
Frequency List
* ranked list of common words (e.g., NGSL)
* determines known vs unknown vocabulary
* stored as static JSON

Frequency Rank
* numeric indicator of word frequency
* lower rank = more common

H
Hard Refresh
* Ctrl+Shift+R to bypass browser cache

Highlighting Logic
* strict priority:
  * green underline = cognate
  * normal = known (frequency or master list)
  * red asterisk = unknown
* implemented in centralized highlight pipeline

L
Lemma
* canonical dictionary form of a word
* examples:
  * running → run
  * went → go
  * mejores → mejor

Lemma Map
* JSON mapping inflected forms to lemmas
* one map per language
* loaded at startup

M
Master List
* curriculum sequence stored in Supabase
* defines allowed vocabulary order
* frontend uses plain strings for masterList

Master List Item
* lemma
* language
* rank
* isCognate flag

N
Normalization
* convert text to NFD form and remove diacritics
* ensures consistent lookup across languages

P
Project
* saved writing session containing text, word list, language, timestamp
* stored in Supabase

project_id
* UUID linking master list rows and project wordlist rows to a specific project

Project Word List
* custom list of lemmas appearing in the current text
* displayed in Column C
* updated dynamically as teacher types

R
Renderer
* function that converts data into UI output
* examples:
  * renderProjectList
  * renderViolationsPanel

T
Tier
* linguistic category for cognates:
  * latin
  * greek
  * biblical
  * general
* used for color-coded highlighting

Tokenizer
* splits text into tokens
* rules:
  * English/Spanish: whitespace + punctuation
  * Greek/Latin: Unicode-aware, accent-aware
* must not trigger highlight or autosave

Token
* single unit of text produced by tokenizer
* example: "running," → "running"

V
Violations Panel
* scrollable diagnostic UI showing curriculum violations in real time
* updated after highlight pipeline runs

W
Writing Window
* single contenteditable editor where text is typed
* words highlighted based on cognates, master list, and frequency list
* rendered using HTML spans with no nested elements

Summary
This glossary defines core terminology used throughout WordList Writer. Update it whenever new concepts, lists, or features are introduced.

Documentation Formatting Reminder
* use plain text section titles
* use asterisks (*) for bullet points
* do not insert blank lines inside bullet lists
* use ASCII-only characters
* avoid Markdown headings (#)
* avoid fenced code blocks unless necessary
* use Step format for workflows to prevent GitHub auto-renumbering
