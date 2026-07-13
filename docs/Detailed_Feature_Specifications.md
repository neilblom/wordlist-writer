# Detailed Feature Specifications
This document provides technical specifications for all major features of the WordList Writer application.  
It describes how each system behaves internally, how data flows through the pipelines, and how UI components interact with underlying logic.

---

## 1. Highlight Pipeline
The highlight pipeline processes user‑entered text and applies visual annotations based on linguistic and curriculum rules.

### 1.1 Tokenization
- Text is split using a regex that preserves punctuation and whitespace:  
  `/(\s+|[.,!?;:"()])/`
- Non‑word tokens (punctuation, whitespace) are passed through unchanged.

### 1.2 Normalization
Each token undergoes:
1. Lowercasing  
2. Apostrophe normalization (`’` → `'`)  
3. Lemma lookup via `lemmaMap`  
4. Canonical normalization via `normalizeLemma()`

### 1.3 Function Word Handling
- Function words are stored in `functionWords` (a Set).
- Function words bypass all curriculum checks and are rendered unchanged.

### 1.4 Cognate Highlighting
- Cognates are stored in `COGNATE_MAP`.
- Each cognate includes:
  - Spanish equivalent (`es`)
  - Cognate tier (`latin`, `greek`, `biblical`, `general`)
- Highlighting uses tier‑specific background colors.
- Cognates render as `<span>` elements with tooltip metadata.

### 1.5 Frequency List Integration
- `frequencyList` contains normalized lemmas from the NGSL/COCA‑based list.
- If a lemma is in the frequency list, the token is rendered unchanged (no asterisk).

### 1.6 Master List Integration
- `masterList` contains teacher‑approved vocabulary.
- A normalized lemma is considered “known” if present in `masterSet`.

### 1.7 Unknown Word Marking
- Unknown words (not in master list) render with a red asterisk:  
  `word<sup style="color:red;">*</sup>`

---

## 2. Curriculum Violations Panel
The Curriculum Violations Panel provides real‑time diagnostic feedback on curriculum mismatches.

### 2.1 Location
- Rendered in `index.html` immediately below `#top-panel`:
  ```html
  <div id="violations-panel"></div>
2.2 Styling
Defined in styles.css:
  #violations-panel {
    max-height: 200px;
    overflow-y: auto;
    border: 1px solid #ccc;
    padding: 8px;
    margin: 10px 0;
    background: #fafafa;
  }
2.3 Data Source
Violations are stored in a global array:
  let curriculumViolations = [];
2.4 Violation Collection
Violations are collected inside renderHighlights() during token processing.

Three violation types are currently supported:
1. Unknown Word
  Lemma not found in master list.
2. Curriculum Gap
  Placeholder: currently identical to Unknown Word.
3. Out‑of‑Order Vocabulary
  Placeholder: reserved for future curriculum sequencing logic.

Each violation is stored as:
  {
    word: tok,
    lemma: normalized,
    type: "...",
    explanation: "..."
  }
2.5 Reset Logic
Before each highlight pass:
  curriculumViolations = [];
2.6 Rendering
The panel is updated via:
  renderViolationsPanel();
The renderer:

Displays “No curriculum violations” when empty.

Otherwise renders a table of violations.

2.7 Popup Removal
The previous curriculum popup warning system has been removed.
The panel replaces all popup‑based curriculum feedback.

3. Master List System
Master List Rendering Requirements
Each master list item must contain:

rank

word

language

optional cognate

optional cognates object

Renderer must write into:
  #master-list-container

Renderer must output 7 grid columns:

rank

word

language

cognate flag

cognates object

edit button

delete button

Violations Panel Requirements
Renderer writes into #violations-panel

Panel must exist in HTML

Panel must be placed below the top panel

Panel updates after every input event

Panel clears when violations disappear

This prevents future “Why is Column D blank?” or “Why is the panel not updating?” confusion

3.1 Structure
Each master list entry includes:

word or english

Optional metadata (tier, notes, etc.)

3.2 Normalization
All master list entries are normalized via normalizeLemma().

3.3 Storage
Stored in masterList (array).

Normalized lemmas stored in masterSet (Set).

3.4 UI
Displayed in Column D (“Master List”).

Supports:

Add

Save

Load

Hover tooltip



4. Project List System
4.1 Purpose
Tracks all lemmas used in the current writing project.

4.2 Storage
Stored in projectListSet (Set).

4.3 Population
Occurs during:
  updateProjectList(text)
4.4 UI
Displayed in Column C (“Project List”).

5. Two‑Layer Editor
5.1 Structure
The editor consists of:

textarea#input-area (raw text input)

div#display-layer (highlighted output)



5.2 Behavior
User types into the textarea.

Highlighted output is rendered in the display layer.

Display layer is visually overlaid but not editable.

5.3 Update Trigger
All updates occur inside:
  inputArea.addEventListener("input", ...)

6. Cognate System
6.1 Data Source
COGNATE_MAP contains:

normalized lemma

Spanish equivalent

cognate tier

6.2 Rendering
Cognates are highlighted with tier‑specific colors and tooltips.

7. Frequency List System
7.1 Data Source
frequencyList contains normalized lemmas from NGSL/COCA‑based lists.

7.2 Behavior
Frequency list words are treated as “known” and rendered without asterisks.

8. Supabase Integration (Phase 2)
  Supabase integration is planned for Phase 2 and will include:
  Cloud project storage
  Cloud master list storage
  User authentication
  Multi‑device sync
  Specifications will be added once Phase 2 begins.

Add/update the section for:
  Save Project
  Load Project
  Save Wordlist (needs refinement)
  Load Wordlist
  new controller behavior:
    .insert([row]).select()
---
- describe the corruption symptoms (duplicates, missing ranks, scrambled ordering)
- describe the deduplication logic (keep lowest rank)
- describe the new validation step:
-   check for duplicates
-   check for missing ranks
-   check for rank continuity (1–1000)
-   check for lemma normalization collisions
- describe the rebuild process from official NGSL source
- describe how ingestion handles malformed lists (fail fast, log error)

This ensures future debugging is easier and prevents regressions.
Frequency List Ingestion Specification (Updated July 2026)
Overview
The frequency list ingestion pipeline imports external vocabulary lists (NGSL‑1K, NGSL‑Full, NAWL, BSL, TSL, etc.) into the Supabase frequency_lists table. Each list must meet strict structural and lexical requirements to ensure consistency across all downstream features (highlighting, curriculum violations, cognate lookup, tooltip metadata, and frequency‑based sorting).

Recent debugging revealed that corrupted NGSL‑1K data can silently break ingestion, even when the database is empty and the service role is correct. This section documents the updated ingestion requirements and validation steps.

🔍 Ingestion Requirements
1. Structural Requirements
Each frequency list must contain unique lemmas after normalization.

NGSL‑1K must contain exactly 1000 unique lemmas.

Ranks must be continuous (1 → N) with no gaps.

Each entry must contain:

rank (integer)

lemma (string)

Lemmas must be normalized using the system’s normalization pipeline (lowercasing, Unicode normalization, punctuation stripping).

🛡️ 2. Pre‑Ingestion Validation (New)
Before inserting any rows into Supabase, the ingestion script must perform:

A. Duplicate Lemma Detection
Detect duplicates after normalization.

If duplicates exist:

Keep the lowest rank (highest frequency).

Discard all higher‑rank duplicates.

Log all duplicates for developer review.

B. Rank Continuity Check
Verify that ranks form a continuous sequence:

NGSL‑1K: 1 → 1000

NGSL‑Full: 1 → 2800

If ranks are missing or out of order:

Fail ingestion immediately.

Log the issue.

C. Normalization Collision Detection
Detect cases where different lemmas normalize to the same string:

e.g., "Call" and "call"

e.g., "co-operate" and "cooperate"

Treat collisions as duplicates.

D. File Integrity Check
Verify that the source file ends cleanly (no truncated JSON).

Verify that the number of entries matches expected list size.

Verify that the file contains no malformed objects.

🧪 3. Ingestion Behavior
A. Batch Insert
Rows are inserted in batches using the Supabase service role.

Unique constraint enforced:

UNIQUE(language_id, lemma, source)

B. Fail‑Fast Strategy
If any validation step fails:

Do not insert partial data.

Log the error.

Abort ingestion.

C. Logging
Log:

number of duplicates removed

number of normalization collisions

number of missing ranks

number of malformed entries

final number of rows inserted

🧭 4. Rationale for the Update
During ingestion debugging (July 2026), NGSL‑1K ingestion repeatedly failed with duplicate key violations despite the database being empty. Investigation revealed:

NGSL‑1K JSON contained dozens of duplicate lemmas

NGSL‑1K JSON was missing ~80 ranks

NGSL‑1K JSON ended at rank 918 instead of 1000

NGSL‑1K JSON had scrambled ordering

NGSL‑1K JSON had been corrupted prior to ingestion

This update ensures the ingestion pipeline is robust against corrupted frequency lists and guarantees that downstream features (highlighting, curriculum violations, cognate lookup, tooltip metadata) operate on clean, validated data.

🧱 5. Developer Notes
Always rebuild NGSL‑1K from the official NGSL source.

Never ingest lists that have not passed validation.

Validation scripts should be runnable independently of ingestion.

Validation should be added to CI in Phase 2 (Supabase integration).
---

9. Future Enhancements (Optional)
These enhancements are not required for current functionality:

Violation grouping

Sorting violations

Collapsible sections

Severity color coding

Toggle button for panel visibility

Jump‑to‑word links

Tier‑aware out‑of‑order detection

These can be added safely in later phases.



