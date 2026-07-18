Detailed Feature Specifications — WordList Writer
Version: 2026-07-17
Status: Authoritative Source of Truth

1. Highlight Pipeline
The highlight pipeline processes user text and applies visual annotations based on linguistic and curriculum rules.

1.1 Tokenization
* Regex preserves punctuation and whitespace: /(\s+|[.,!?;:"()])/
* Non-word tokens pass through unchanged

1.2 Normalization
Step 1: Lowercase  
Step 2: Normalize apostrophes  
Step 3: Lemma lookup via lemmaMap  
Step 4: Canonical normalization via normalizeLemma()

1.3 Function Words
* Stored in functionWords (Set)
* Bypass curriculum checks
* Render unchanged

1.4 Cognate Highlighting
* Cognates stored in COGNATE_MAP
* Each entry includes Spanish equivalent and tier
* Tier colors applied globally
* Rendered as span elements with tooltip metadata

1.5 Frequency List Integration
* frequencyList contains normalized NGSL/COCA lemmas
* Frequency words render as known (black)

1.6 Master List Integration
* masterList contains teacher-approved vocabulary
* masterSet stores normalized lemmas
* Known words render black

1.7 Unknown Word Marking
* Unknown words render with red asterisk: word<sup style="color:red;">*</sup>

2. Curriculum Violations Panel
Provides real-time diagnostic feedback on curriculum mismatches.

2.1 Location
* Rendered below #top-panel in index.html
* Element: #violations-panel

2.2 Styling
* max-height: 200px
* overflow-y: auto
* border: 1px solid #ccc
* padding: 8px
* background: #fafafa

2.3 Data Source
* curriculumViolations (array)

2.4 Violation Types
* Unknown Word
* Curriculum Gap
* Out-of-Order Vocabulary (future)

2.5 Reset Logic
* curriculumViolations = [] before each highlight pass

2.6 Rendering
* renderViolationsPanel()
* Shows “No curriculum violations” when empty
* Otherwise displays table of violations

2.7 Popup Removal
* Popup warning system removed
* Panel replaces all popup-based feedback

3. Master List System
Master List defines the curriculum sequence and appears in Column D.

3.1 Rendering Requirements
* rank
* word
* language
* cognate flag
* cognates object
* edit button
* delete button
* Render into #master-list-container

3.2 Structure
* word or english
* optional metadata (tier, notes)

3.3 Normalization
* All entries normalized via normalizeLemma()

3.4 Storage
* masterList (array)
* masterSet (Set)

3.5 UI
* Column D displays Master List
* Supports add, save, load, hover tooltip

Master List Data Model
Frontend shape:
* word
* english
* rank
* language
* length

Backend shape (Supabase):
* lemma
* rank
* language
* is_cognate
* project_id

4. Project List System
Tracks lemmas used in current project.

4.1 Purpose
* Shows vocabulary used in text

4.2 Storage
* projectListSet (Set)

4.3 Population
* updateProjectList(text)

4.4 UI
* Column C displays Project List

5. Two-Layer Editor
5.1 Structure
* textarea#input-area (raw text)
* div#display-layer (highlighted output)

5.2 Behavior
* User types into textarea
* Highlighted output rendered in display layer
* Display layer is non-editable overlay

5.3 Update Trigger
* inputArea.addEventListener("input", ...)

6. Cognate System
6.1 Data Source
* COGNATE_MAP contains normalized lemma, Spanish equivalent, tier

6.2 Rendering
* Tier-specific colors
* Tooltip metadata

7. Frequency List System
7.1 Data Source
* frequencyList contains normalized NGSL/COCA lemmas

7.2 Behavior
* Frequency words treated as known

8. Supabase Integration (Phase 2)
Supabase handles project and master list persistence.

8.1 Save Project
* Save project metadata
* Save project wordlist
* Save master list updates

8.2 Load Project
* Load project metadata
* Load project wordlist
* Reconstruct UI state

8.3 Save Wordlist
* Requires projectId
* Must delete old rows before inserting new ones
* Must use .select() to return inserted rows

8.4 Load Wordlist
* Convert Supabase rows back into frontend shape
* UI rendering depends on word, not lemma

8.5 Controller Behavior
* .insert([row]).select() required
* Map word → lemma on save
* Map lemma → word on load

9. Frequency List Ingestion Specification (Updated July 2026)
Ensures clean, validated frequency lists before ingestion.

9.1 Structural Requirements
* Unique lemmas after normalization
* NGSL-1K must contain exactly 1000 lemmas
* Ranks must be continuous
* Each entry must contain rank and lemma
* Lemmas normalized consistently

9.2 Pre-Ingestion Validation
Duplicate Detection
* Detect duplicates after normalization
* Keep lowest rank
* Discard higher-rank duplicates
* Log duplicates

Rank Continuity Check
* Verify continuous sequence (1 → N)
* Fail ingestion if missing ranks

Normalization Collision Detection
* Detect collisions (e.g., Call vs call)
* Treat collisions as duplicates

File Integrity Check
* Verify clean file ending
* Verify correct number of entries
* Verify no malformed objects

9.3 Ingestion Behavior
Batch Insert
* Insert rows in batches
* Unique constraint: (language_id, lemma, source)

Fail-Fast Strategy
* Abort ingestion on any validation failure
* Log error
* No partial inserts

Logging
* Number of duplicates removed
* Number of collisions
* Number of missing ranks
* Number of malformed entries
* Final rows inserted

9.4 Rationale
* Corrupted NGSL-1K caused ingestion failures
* Missing ranks, scrambled ordering, truncated file
* Validation prevents regressions

9.5 Developer Notes
* Always rebuild NGSL-1K from official source
* Never ingest unvalidated lists
* Validation scripts must run independently
* Add validation to CI in Phase 2

10. Future Enhancements (Optional)
* Violation grouping
* Sorting violations
* Collapsible sections
* Severity color coding
* Toggle button
* Jump-to-word links
* Tier-aware out-of-order detection

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
