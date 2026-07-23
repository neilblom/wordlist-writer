Master List Workflow — WordList Writer
Version: 2026-07-23
Status: Authoritative Workflow Guide

Overview
This document explains how to use the Master List (Column D) inside WordList Writer. It merges the original curriculum workflow with updated Phase 2 architecture rules, tokenizer rules, rendering rules, highlight pipeline rules, hybrid master list model, July 19 fixes, and the simplified cognate system. The Master List defines curriculum order, controls highlighting, and supports project-level curriculum development. WordList Writer is a teacher-facing authoring tool.

Purpose of the Master List
The Master List is a pedagogical sequence of lemmas. It defines the order in which vocabulary should appear in beginner-level texts. It is not alphabetical and not frequency-ordered. It is a curriculum.
The Master List supports:
* curriculum alignment
* controlled text creation
* cognate scaffolding
* frequency awareness
* project-level diagnostics

Where the Master List Appears
Column D in the top panel displays the Master List.
Each row shows:
* rank
* word (lemma)
* language
* edit button
* delete button
The writing window uses this list to determine highlight classes and curriculum-order violations.

How Highlighting Uses the Master List
Highlight priority:
* green underline = cognate
* normal = known (frequency list or master list)
* red asterisk = unknown (not in master list)
Curriculum-order violations occur when:
* a lemma appears before its assigned rank
* a lemma is not in the master list at all
Violations appear in the Violations Panel.

Adding Lemmas to the Master List
Teachers can add lemmas directly inside Column D.
Supported actions:
* add new lemma
* insert lemma at specific rank
* reorder lemmas
All changes update:
* masterList array
* masterSet (normalized)
* highlight pipeline via requestHighlightUpdate
* project list
* Supabase storage

Cognate Insertion Workflow (Phase 2 Simple Cognate Window)
When a teacher clicks a cognate in Column B:
Step 1: matching tokens in the writing window highlight green
Step 2: the English lemma is inserted into the Master List
Step 3: Column D re-renders
Step 4: requestHighlightUpdate runs the highlight pipeline
Phase 2 constraints:
* English-only insertion
* no dictionary profiles
* no alphabetical dictionary
* no pending or official cognates
* no publish workflow
* no cross-language equivalents

Typed Word Insertion Workflow
When a teacher types a new lemma in the writing window:
Step 1: the lemma is normalized
Step 2: if not in the Master List, it is marked red
Step 3: teacher may choose to add it to the Master List
Step 4: it is inserted after the last lemma appearing in the story
Step 5: requestHighlightUpdate runs the highlight pipeline

Curriculum-Order Logic
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

Hybrid Master List Model
The curriculum system uses four coordinated vocabulary layers:
* global master list defines universal curriculum backbone
* language master lists provide frequency ranking
* context master lists provide optional curriculum modes
* project master lists track vocabulary progression inside a single story
The system compares vocabulary across layers to enforce curriculum rules.

Phase 2 Master List Constraints
* master list stores English lemmas only
* stored values are normalized strings
* UI displays capitalized English labels
* no multilingual insertion
* no canonical switching
* no cognate objects
* no tier metadata
* no dictionary profiles

Saving and Loading the Master List
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

Updated Architecture Rules
Master List rules:
* masterList contains plain strings only
* master list rendering occurs after highlight
* master list updates must not call handleStableInput directly
Project list rules:
* updateProjectList must not trigger highlight
* updateProjectList runs after highlight on startup
Frequency known-word rules:
* known words come from NGSL-1K
* master list is not a known-word list
Highlight pipeline rules:
* all highlight operations must enter through requestHighlightUpdate
* no highlight during IME composition
* requestHighlightUpdate uses debounce
Startup rules:
* startup triggers highlight once
* project list and order check run after highlight
* recommended delay is 75ms
Cognate system rules (Phase 2):
* cognate updates must call requestHighlightUpdate
* English lemma only is inserted
* no merged dictionary
* no alphabetical dictionary

Best Practices for Teachers
* keep the Master List small and focused
* add lemmas only when pedagogically necessary
* use cognates to scaffold new vocabulary
* check the Violations Panel frequently
* maintain consistent ordering
* use frequency metadata to guide difficulty

Best Practices for Developers
* always normalize lemmas before insertion
* ensure masterSet stays in sync with masterList
* update highlight pipeline using requestHighlightUpdate
* validate rank continuity
* guard against null values in normalizeLemma
* ensure saveEverything sends correct shape
* ensure load pipeline assigns plain strings correctly

Summary
The Master List is the backbone of WordList Writer. It defines curriculum order, controls highlighting, and enables teachers to build pedagogically sequenced texts. This updated workflow reflects correct Phase 2 behavior: English-only master list, simple cognate window, stable tokenizer behavior, and predictable highlight operations. All Phase 3 cognate dictionary architecture has been removed to maintain stability.
