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

Alphabetical Dictionary
* bottom section of Column B
* displays all cognates for the active dictionary profile
* sorted alphabetically
* includes official and pending cognates
* rebuilt from merged dictionary

C
Cognate
* a word in another language sharing a historical root with an English word
* highlighted green (highest priority)
* examples:
  * information ↔ informacion
  * manual ↔ manus
  * biology ↔ bios

Cognate Tier
* metadata describing cognate category
* tiers:
  * latin
  * greek
  * biblical
  * general
* used for color-coded highlighting

Cognates (Official)
* published cognate entries stored in Supabase
* used for highlight pipeline
* used for alphabetical dictionary

Cognates (Pending)
* newly added or edited cognates awaiting publication
* override official entries in merged dictionary
* stored in Supabase

Cognate Window
* Column B in the top panel
* contains detected cognates and alphabetical dictionary
* clicking highlights matching tokens and inserts cognate into master list

COGNATE_MAP (Deprecated)
* original static cognate map
* replaced by mergedCognateMap

Collector
* logic inside highlight pipeline that pushes violation objects into curriculumViolations

Curriculum Violation
* mismatch between text and curriculum rules
* types:
  * out-of-order vocabulary
  * unknown word
  * curriculum gap

Cross-Language Master List
* Supabase table storing English, Spanish, Latin, and Greek equivalents
* includes lemmas, cognate flags, frequency ranks, shared roots

D
Dictionary Profile
* per-project setting determining which cognates appear in Column B
* profiles:
  * spanish
  * latin
  * greek
  * merged
* affects cognate filtering only
* stored in Supabase

Detected Cognates
* top section of Column B
* shows cognates present in the current text
* filtered by dictionary profile

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

Hybrid Cognate Window
* combined UI showing detected cognates and alphabetical dictionary
* rebuilt from merged dictionary
* filtered by dictionary profile

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

Merged Dictionary
* unified cognate map built from official + pending entries
* pending entries override official entries
* used for highlight pipeline
* used for hybrid cognate window
* used for alphabetical dictionary

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

Publish Cognates
* action that merges pending cognates into official table
* clears pending table
* rebuilds merged dictionary

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
This glossary defines core terminology used throughout WordList Writer. Update it whenever new concepts, lists, or features are introduced.

Documentation Formatting Reminder
* use plain text section titles
* use asterisks (*) for bullet points
* do not insert blank lines inside bullet lists
* use ASCII-only characters
* avoid Markdown headings (#)
* avoid fenced code blocks unless necessary
* use Step format for workflows to prevent GitHub auto-renumbering
