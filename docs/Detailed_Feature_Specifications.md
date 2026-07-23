Detailed Feature Specifications — WordList Writer
Version: 2026-07-20
Status: Authoritative Source of Truth

Overview
This document defines the internal behavior of all major systems in WordList Writer. It merges the original feature specifications with updated architecture rules, tokenizer rules, rendering rules, highlight pipeline rules, dictionary profile system, hybrid cognate window, alphabetical dictionary, cognate merging rules, Supabase schema updates, and July 19 fixes. It is the authoritative reference for implementing or debugging core features.

1. Highlight Pipeline
The highlight pipeline processes user text and applies visual annotations based on linguistic and curriculum rules. All highlight operations must enter through requestHighlightUpdate.

1.1 Tokenization (Updated)
* tokenizeUnified splits text into tokens
* whitespace and punctuation preserved as raw tokens
* normalized tokens not logged
* tokenizer must not trigger highlight or autosave
* tokenizer must preserve spacing and punctuation
* tokenizer must return objects only for word tokens

1.2 Normalization (Updated)
Step 1: lowercase  
Step 2: Unicode NFD  
Step 3: strip diacritics  
Step 4: remove punctuation for lemma lookup  
Step 5: lemma lookup via lemmaMap  
Step 6: canonical normalization via normalizeLemma()

1.3 Function Words
* stored in functionWords (Set)
* bypass curriculum checks
* rendered unchanged

1.4 Cognate Highlighting (Updated)
* cognates stored in mergedCognateMap
* mergedCognateMap built from official + pending cognates
* dictionaryProfile filters which cognates apply
* each entry includes tier metadata
* tier colors applied globally
* rendered as span elements with tooltip metadata
* cognate highlighting overrides all other highlight types

1.5 Frequency List Integration (Updated)
* frequencySet contains normalized NGSL lemmas
* frequency words render as known (normal)
* NGSL-1K defines known-word baseline

1.6 Master List Integration (Updated)
* masterList contains plain strings only
* masterSet stores normalized lemmas
* known words render normal
* master list updates must trigger requestHighlightUpdate

1.7 Unknown Word Marking (Updated)
* unknown words render with red asterisk
* unknown detection occurs after cognate and known checks

1.8 Rendering Rules (Updated)
* single-layer editor
* no overlay
* no nested spans
* editor.innerHTML replaced on each highlight pass
* highlight pipeline must not mutate DOM after render

2. Curriculum Violations Panel
Provides real-time diagnostic feedback on curriculum mismatches.

2.1 Location
* rendered below #top-panel
* element: #violations-panel

2.2 Styling
* max-height: 200px
* overflow-y: auto
* border: 1px solid #ccc
* padding: 8px
* background: #fafafa

2.3 Data Source
* curriculumViolations (array)

2.4 Violation Types (Updated)
* unknown word
* curriculum gap
* out-of-order vocabulary

2.5 Reset Logic
* curriculumViolations = [] before each highlight pass

2.6 Rendering
* renderViolationsPanel()
* shows “No curriculum violations” when empty
* otherwise displays table of violations

2.7 Popup Removal
* popup warning system removed
* panel replaces all popup-based feedback

3. Master List System (Updated)
Master List defines the curriculum sequence and appears in Column D.

3.1 Rendering Requirements
* rank
* lemma
* language
* isCognate flag
* edit button
* delete button
* rendered into #master-list-container

3.2 Structure (Updated)
* plain strings only
* no multilingual fields until Phase 3

3.3 Normalization
* all entries normalized via normalizeLemma()

3.4 Storage
* masterList (array)
* masterSet (Set)

3.5 UI
* Column D displays master list
* supports add, insert, reorder, delete
* updates highlighting immediately

Master List Data Model
Frontend shape:
* lemma
* rank
* language
Backend shape (Supabase):
* lemma
* rank
* language
* is_cognate
* project_id

4. Project List System (Updated)
Tracks lemmas used in current project.

4.1 Purpose
* shows vocabulary used in text

4.2 Storage
* projectListSet (Set)

4.3 Population
* updateProjectList(text)
* must not trigger highlight

4.4 UI
* Column C displays project list

5. Editor Architecture (Updated)
5.1 Structure
* single contenteditable editor
* no overlay
* no textarea + display-layer split

5.2 Behavior
* user types directly into editor
* highlight pipeline rewrites editor.innerHTML

5.3 Update Trigger
* input event triggers requestHighlightUpdate
* IME composition events suppress highlight

6. Cognate System (Updated)
6.1 Data Source
* mergedCognateMap contains normalized lemma, cognate, tier, profile
* built from cognates_official + cognates_pending

6.2 Rendering
* tier-specific colors
* tooltip metadata
* cognate click inserts lemma into master list

6.3 Dictionary Profiles
* profiles: spanish, latin, greek, merged
* profile stored per project
* profile filters cognates in detected and alphabetical sections

6.4 Hybrid Cognate Window
Detected section:
* shows cognates present in current text
Alphabetical dictionary:
* shows all cognates for active profile
* sorted alphabetically
* includes official and pending entries

6.5 Cognate Merging
* pending entries override official entries
* merged dictionary rebuilt after publish
* merged dictionary used for highlight pipeline

7. Frequency List System (Updated)
7.1 Data Source
* frequencySet contains normalized NGSL lemmas

7.2 Behavior
* frequency words treated as known
* NGSL-1K defines known baseline

8. Supabase Integration (Updated)
Supabase handles project, master list, cognate, and profile persistence.

8.1 Save Project
* save project metadata
* save dictionary profile
* return id via .select()
* update project-id-input

8.2 Load Project
* load project metadata
* load dictionary profile
* load project wordlist
* load master list
* load cognates_official
* load cognates_pending
* rebuild merged dictionary
* reconstruct UI state
* trigger highlight

8.3 Save Wordlist
* requires projectId
* delete old rows before inserting new ones
* use .insert([...]).select()

8.4 Load Wordlist
* convert Supabase rows into frontend shape
* UI rendering depends on lemma

8.5 Controller Behavior
* map lemma → lemma on save
* map lemma → lemma on load
* normalizeLemma must guard against null

9. Frequency List Ingestion Specification (Updated)
Ensures clean, validated frequency lists before ingestion.

9.1 Structural Requirements
* unique lemmas after normalization
* NGSL-1K must contain exactly 1000 lemmas
* ranks must be continuous
* each entry must contain rank and lemma
* lemmas normalized consistently

9.2 Pre-Ingestion Validation
Duplicate Detection
* detect duplicates after normalization
* keep lowest rank
* discard higher-rank duplicates

Rank Continuity Check
* verify continuous sequence (1 → N)
* fail ingestion if missing ranks

Normalization Collision Detection
* detect collisions
* treat collisions as duplicates

File Integrity Check
* verify clean file ending
* verify correct number of entries
* verify no malformed objects

9.3 Ingestion Behavior
Batch Insert
* insert rows in batches
* unique constraint: (language_id, lemma, source)

Fail-Fast Strategy
* abort ingestion on any validation failure
* no partial inserts

Logging
* duplicates removed
* collisions
* missing ranks
* malformed entries
* final rows inserted

9.4 Rationale
* corrupted NGSL-1K caused ingestion failures
* validation prevents regressions

9.5 Developer Notes
* always rebuild NGSL-1K from official source
* never ingest unvalidated lists
* validation scripts must run independently

10. Startup Sequence (Updated)
Step 1: load language and cognates  
Step 2: load dictionary profile  
Step 3: load project text  
Step 4: load master list  
Step 5: load project wordlist  
Step 6: load violations  
Step 7: trigger highlight once  
Step 8: after 75ms run project list and order check  
Step 9: display “Load complete”

11. Future Enhancements (Optional)
* violation grouping
* sorting violations
* collapsible sections
* severity color coding
* toggle button
* jump-to-word links
* tier-aware out-of-order detection

Documentation Formatting Reminder
* use plain text section titles
* use asterisks (*) for bullet points
* do not insert blank lines inside bullet lists
* use ASCII-only characters
* avoid Markdown headings (#)
* avoid fenced code blocks unless necessary
* use Step format for workflows to prevent GitHub auto-renumbering
