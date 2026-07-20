Master_List_Workflow — WordList Writer
Version: 2026-07-20
Status: Authoritative Workflow Guide

Overview
This document explains how to use the Master List (Column D) inside WordList Writer. It merges the original curriculum workflow with updated architecture rules, tokenizer rules, rendering rules, highlight pipeline rules, and July 19 fixes. The Master List defines curriculum order, controls highlighting, and supports multilingual expansion. This workflow guide is designed for teachers and for future development sessions.

1. Purpose of the Master List
The Master List is a pedagogical sequence of lemmas. It defines the order in which vocabulary should appear in beginner-level texts. It is not alphabetical and not frequency-ordered. It is a curriculum.
The Master List supports:
* curriculum alignment
* controlled text creation
* cross-language vocabulary modeling
* cognate scaffolding
* frequency awareness
* project-level diagnostics

2. Where the Master List Appears
Column D in the top panel displays the Master List.
Each row shows:
* rank
* word (lemma)
* language
* cognate flag
* edit button
* delete button
The writing window uses this list to determine highlight classes and curriculum-order violations.

3. How Highlighting Uses the Master List
Highlight priority:
* green underline = cognate
* normal = known (frequency list or master list)
* red asterisk = unknown (not in master list)
Curriculum-order violations occur when:
* a lemma appears before its assigned rank
* a lemma is not in the master list at all
Violations appear in the Violations Panel.

4. Adding Lemmas to the Master List
Teachers can add lemmas directly inside Column D.
Supported actions:
* add new lemma
* insert lemma at specific rank
* reorder lemmas
* edit cross-language equivalents
* update cognate flags
All changes update:
* masterList array
* masterSet (normalized)
* highlight pipeline via requestHighlightUpdate
* project list
* Supabase storage

5. Cognate Insertion Workflow
When a teacher clicks a cognate in Column B:
Step 1: matching tokens in the writing window highlight green
Step 2: the cognate is inserted into the Master List
Step 3: the correct language is assigned
Step 4: Column D re-renders
Step 5: requestHighlightUpdate runs the highlight pipeline
This workflow supports discovery → selection → curriculum building.

6. Typed Word Insertion Workflow
When a teacher types a new lemma in the writing window:
Step 1: the lemma is normalized
Step 2: if not in the Master List, it is marked red
Step 3: teacher may choose to add it to the Master List
Step 4: it is inserted after the last lemma appearing in the story
Step 5: requestHighlightUpdate runs the highlight pipeline
This supports dynamic curriculum development.

7. Curriculum-Order Logic
The Master List defines the allowed sequence.
Example:
* Rank 1: the
* Rank 2: and
* Rank 3: man
* Rank 4: woman
If “woman” appears before “man,” the Violations Panel shows:
* out-of-order vocabulary
If “child” appears and is not in the list:
* unknown word

8. Cross-Language Workflow (Future Phases)
The Master List will eventually support:
* english
* spanish
* latin
* greek
Each lemma may include:
* cognates object
* frequency metadata
* tier metadata
Cross-language equivalents allow teachers to build unified curricula.

9. Saving and Loading the Master List
Save Workflow:
Step 1: teacher clicks Save Project
Step 2: masterList is treated as an array of plain strings
Step 3: rows inserted into Supabase as { project_id, lemma }
Step 4: old rows deleted
Step 5: Supabase returns inserted rows
Load Workflow:
Step 1: teacher clicks Load Project
Step 2: rows loaded from Supabase
Step 3: masterList assigned directly from returned data
Step 4: Column D re-renders
Step 5: requestHighlightUpdate runs the highlight pipeline

10. Updated Architecture Rules
Master List rules:
* masterList contains plain strings only
* master list rendering occurs after highlight
* master list updates must not call handleStableInput directly
* updateProjectList must not trigger highlight
* checkStoryOrderAgainstMaster must not trigger highlight
Project ID rules:
* project-id-input must update on create, load, save, and new project
* incorrect project ID causes empty master list loads
Frequency known-word rules:
* known words come from NGSL-1K
* master list is not a known-word list
* correct known-word set uses normalized frequency lemmas
Highlight pipeline rules:
* all highlight operations must enter through requestHighlightUpdate
* no highlight during IME composition
* requestHighlightUpdate uses debounce
Startup rules:
* startup triggers highlight once
* project list and order check run after highlight
* recommended delay is 75ms

11. Best Practices for Teachers
* keep the Master List small and focused
* add lemmas only when pedagogically necessary
* use cognates to scaffold new vocabulary
* check the Violations Panel frequently
* maintain consistent ordering
* use frequency metadata to guide difficulty
* build cross-language equivalents gradually

12. Best Practices for Developers
* always normalize lemmas before insertion
* ensure masterSet stays in sync with masterList
* update highlight pipeline using requestHighlightUpdate
* validate rank continuity
* guard against null values in normalizeLemma
* ensure saveEverything sends correct shape
* ensure load pipeline assigns plain strings correctly

13. Summary
The Master List is the backbone of WordList Writer. It defines curriculum order, controls highlighting, supports multilingual expansion, and enables teachers to build pedagogically sequenced texts. This updated workflow merges the original design with new architecture rules and July 19 fixes to ensure stable behavior across the entire system.
