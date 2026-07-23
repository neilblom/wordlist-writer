Glossary — WordList Writer
Version: 2026-07-23
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

Cognate Window (Phase 2)
* Column B in the top panel
* displays detected cognates only
* clicking highlights matching tokens and inserts English lemma into master list
* no alphabetical dictionary
* no dictionary profiles
* no pending or official cognates

Collector
* logic inside highlight pipeline that pushes violation objects into curriculumViolations

Curriculum Violation
* mismatch between text and curriculum rules
* types:
  * out-of-order vocabulary
  * unknown word
  * curriculum gap

D
Detected Cognates (Phase 2)
* top section of Column B
* shows cognates present in the current text
* detected using normalized lemmas only
* no profile filtering

F
Frequency List
* ranked list of common words (e.g., NGSL)
* determines known vs unknown vocabulary
* stored as static JSON

Frequency Rank
* numeric indicator of word frequency
* lower rank = more common

G
Green Highlight
* applied to cognates
* highest priority highlight class

H
Hard Refresh
* Ctrl+Shift+R to bypass browser cache

Highlight Pipeline
* centralized system that computes highlight classes
* strict priority:
  * green underline = cognate
  * normal = known (frequency or master list)
  * red asterisk = unknown
* must run only through requestHighlightUpdate

Hybrid Master List Model
* global master list defines universal curriculum backbone
* language master lists provide frequency ranking
* context master lists provide optional curriculum modes
* project master lists track vocabulary progression inside a single story

I
IME Composition
* input method editor events for languages requiring composition
* tokenizer and highlight pipeline must not run during composition

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
* English-only in Phase 2

Master List Item (Phase 2)
* lemma
* language (English only)
* rank

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

Red Asterisk
* highlight applied to unknown words
* lowest priority highlight class

S
Supabase v2
* backend used for persistence
* requires .select() to retrieve inserted rows
* inserts return minimal by default

T
Tokenizer
* splits text into tokens
* rules:
  * English/Spanish: whitespace + punctuation
  * Greek/Latin: Unicode-aware, accent-aware
* must not trigger highlight or autosave

Token
* single unit of text produced by tokenizer
* example: "running," → "running"

U
Unknown Word
* lemma not found in frequencySet or masterSet
* rendered with red asterisk
* triggers curriculum violation

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
This glossary defines core terminology used throughout WordList Writer. All Phase 3 dictionary profile, alphabetical dictionary, merged dictionary, and cognate publishing features have been removed. Update this glossary whenever new concepts or features are introduced.

Documentation Formatting Reminder
* use plain text section titles
* use asterisks (*) for bullet points
* do not insert blank lines inside bullet lists
* use ASCII-only characters
* avoid Markdown headings (#)
* avoid fenced code blocks unless necessary
* use Step format for workflows to prevent GitHub auto-renumbering
