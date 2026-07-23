Product Requirements Document (PRD) — WordList Writer
Version: 2026-07-23
Status: Authoritative Source of Truth
Owner: Neil Blom
Scope: Teacher-Only Vocabulary Authoring Tool

Overview
WordList Writer is a teacher-facing authoring tool for creating controlled beginner-level English texts. Teachers use the tool to craft texts aligned with curriculum order, frequency constraints, simple cognate scaffolding, and lemma normalization. Students never interact with the app. The final output delivered to students is a clean text without highlights or diagnostics.

The tool supports two instructional goals:
* building a beginner-level master vocabulary list
* supporting curriculum-aligned text creation

WordList Writer provides real-time vocabulary analysis, curriculum diagnostics, simple cognate scaffolding, frequency awareness, and project-level vocabulary tracking. All analysis exists to support teacher workflow.

1. Purpose and Scope
WordList Writer helps instructors write linguistically appropriate, pedagogically sequenced texts. It highlights vocabulary usage, checks frequency alignment, identifies simple Spanish → English cognates, and surfaces curriculum-order violations. It supports a single English master list during Phase 2.

Non-Goals:
* no student accounts
* no quizzes or gamification
* no LMS integration
* no student-facing UI
* no automated grading
* no multilingual pipelines (deferred to Phase 3)
* no dictionary profiles or tier metadata

2. Core Features

2.1 Writing Window
* single contenteditable editor
* real-time tokenization
* lemma normalization
* strict highlight priority:
  * green underline = cognate
  * normal = known (frequency or master list)
  * red asterisk = unknown
* export clean text (no highlights)
* IME-safe input handling

2.2 Top Panel (Four Columns)
Column A: Frequency List
* displays NGSL frequency list
* sorted by rank
* click to highlight in writing window

Column B: Cognates (Simple Window)
* Spanish → English cognates only
* click to highlight and insert into Master List

Column C: Project Word List
* tracks lemmas appearing in current text
* normalized before insertion
* shows frequency tier
* stored in Supabase

Column D: Master List
* stable curriculum sequence
* fields:
  * rank
  * lemma
  * language (English only)
* red = not in Master List or used too early
* editable: add, insert, reorder
* updates highlighting immediately

2.3 Violations Panel
Reports:
* out-of-order vocabulary
* unknown words
* curriculum gaps
Behavior:
* updates automatically
* never interrupts writing

3. Language Capability (Phase 2)
Supported language:
* English only

Static JSON modules:
* frequency/english_ngsl.json
* lemmas/english.json
* cognates/english_spanish.json

Multilingual support is deferred to Phase 3.

4. Highlighting Logic
Step 1: cognate
Step 2: master list
Step 3: frequency list
Step 4: unknown

Normalization:
* lowercase
* NFD Unicode normalization
* accent stripping
* punctuation removal

5. Word Lists

5.1 Frequency Lists
Requirements:
* NGSL-1K must contain exactly 1000 unique lemmas
* unique lemma requirement after normalization
* rank continuity required
* fail-fast ingestion
Validation detects:
* duplicates
* missing ranks
* discontinuities
* malformed entries
* normalization collisions
* truncated files

5.2 Lemma Maps
* inflected → lemma
* normalized consistently

5.3 Cognate Lists (Phase 2)
* Spanish → English only
* no tier metadata
* no dictionary profiles
* no alphabetical dictionary

5.4 Project Word Lists (Supabase)
* stored per project
* normalized lemmas
* frequency tier displayed

5.5 Master Lists (Supabase)
Tracks:
* lemma
* rank
* language (English only)
Curriculum ordering:
* master list defines pedagogical sequence
* insert new words after last used master list word
Order warning:
* detects out-of-order usage
* shows expected lemma and suggested correction

6. Architecture Overview

Frontend:
* HTML/CSS/JS
* single contenteditable editor
* centralized highlight pipeline
* tokenizer, normalization, cognate detection, frequency detection
* project list and master list rendering
* IME-safe input handling

Backend (Node.js + Express):
* serves static frontend
* serves JSON lists
* API endpoints for project and master list operations

Database (Supabase):
Tables:
* projects
* project_wordlists
* master_wordlists
Persistence:
* UUIDs generated client-side
* .select() required for all inserts

7. Updated Architecture Rules

Highlight Pipeline:
* all highlight operations must enter through requestHighlightUpdate
* requestHighlightUpdate uses debounce
* no highlight during IME composition
* handleStableInput must never be called directly

Startup Sequence:
Step 1: load language and cognates
Step 2: load project text
Step 3: load master list
Step 4: load project wordlist
Step 5: load violations
Step 6: trigger highlight once
Step 7: after 75ms run project list and order check
Step 8: display "Load complete"

Tokenizer Rules:
* tokenizeUnified must not trigger highlight or autosave
* no normalized token logs
* punctuation and whitespace remain raw strings

Rendering Rules:
* single-layer editor
* no overlay
* no nested spans
* editor.innerHTML replaced on each highlight pass

Project ID Rules:
* project-id-input must update on create, load, save, and new project
* incorrect project ID causes empty master list loads

Master List Rules:
* masterList contains plain strings only
* master list updates must trigger requestHighlightUpdate

Frequency Rules:
* known words come from NGSL-1K
* frequencySet contains normalized lemmas

Autosave Rules:
* autosave must be debounced
* autosave pauses during project resets

8. Save and Load Pipelines

Save Pipeline:
Step 1: save project metadata
Step 2: capture project id
Step 3: save project wordlist
Step 4: save master list

Load Pipeline:
Step 1: load project metadata
Step 2: load project wordlist
Step 3: load master list
Step 4: reconstruct UI state
Step 5: trigger highlight

9. Roadmap (Concise)

Phase 1: English-only rebuild  
Phase 2: Supabase integration  
Phase 3: curriculum modeling (future)  
Phase 4: multiple curriculum support (future)  
Phase 5: teacher workflow enhancements (future)  
Phase 6: optional enhancements (future)

10. Summary
WordList Writer is a teacher-facing authoring tool that provides curriculum-aligned vocabulary control, frequency awareness, simple cognate scaffolding, and lemma normalization. This Phase 2 PRD merges the original pedagogical intent with updated architecture rules, pipelines, workflows, and July 19 fixes. It defines the functional requirements, constraints, and roadmap for WordList Writer as a professional tool for curriculum-aligned text development.
