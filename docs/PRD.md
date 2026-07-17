Product Requirements Document (PRD) — WordList Writer
Version: 2026-07-17
Status: Authoritative Source of Truth
Owner: Neil Blom
Scope: Teacher-Only Vocabulary Authoring Tool

PRD.md — WordList Writer (Node.js + Express + Supabase)

PRD Introduction WordList Writer is a teacher‑facing authoring tool for creating controlled texts for beginner learners across multiple languages. It provides instructors with real‑time vocabulary analysis, curriculum alignment feedback, cognate scaffolding, and frequency‑based highlighting. Students never interact with the app directly; instead, teachers use WordList Writer to craft texts that are pedagogically sequenced, linguistically appropriate, and aligned with a chosen curriculum or master vocabulary list.

The tool supports two major instructional goals. First, it helps determine whether a unified beginner‑level master vocabulary list can be built across English, Spanish, Koine Greek, and Latin. Second, if a unified list is not feasible, WordList Writer enables teachers to create multiple master vocabulary lists tailored to different learner profiles—such as age group, subject matter, or target language—while maintaining consistent analysis and feedback across all lists.

WordList Writer is designed to be flexible, extensible, and grounded in linguistic accuracy. Its normalization pipeline ensures consistent lemma handling across languages, while its cognate and frequency systems provide teachers with actionable insights during text creation. The final output delivered to students is a clean, controlled text without any analytical overlays.

PRD Requirements Functional Requirements 1. Text Analysis - The system must tokenize input text and normalize lemmas across supported languages. - The system must highlight words according to curriculum status (known, unknown, out‑of‑order). - The system must detect cognates using a tier‑based cognate map. - The system must detect frequency tier (NGSL, NAWL, or other lists). - The system must display tooltips with lemma, cognate tier, frequency rank, and curriculum status. 2. Master Vocabulary Lists - The system must load a master vocabulary list from local or cloud storage. - The system must support multiple master lists (age‑based, subject‑based, language‑based). - The system must allow switching between master lists without reloading the app. - The system must validate text against the active master list. - The system must detect curriculum‑order violations. 3. Cognate System - The system must load cognate lists for English‑Spanish, English‑Latin, and English‑Greek. - The system must support tiered cognate classification (general, latin, greek, biblical). - The system must allow expansion of cognate lists over time. - The system must apply cognate detection consistently across highlight loop, tooltip, and project list. 4. Frequency System - The system must load frequency lists (NGSL, NAWL, or custom lists). - The system must detect frequency tier for each lemma. - The system must display frequency information in tooltips and project list. 5. Project List - The system must track all lemmas appearing in the current text. - The system must normalize lemmas before adding them to the project list. - The system must display cognate tier and frequency tier for each lemma. - The system must update dynamically as the teacher edits the text. 6. Teacher Workflow - The system must provide a clean, teacher‑facing UI. - The system must allow exporting a clean version of the text (no highlights). - The system must provide curriculum diagnostics (unknown words, out‑of‑order words). - The system must provide a text difficulty summary (planned for later phases). - The system must support multi‑language authoring.

Non‑Functional Requirements 1. Performance - Text analysis must occur in real time as the teacher types. - Highlighting must update without noticeable delay. 2. Reliability - The system must handle large texts without crashing. - The system must maintain consistent normalization across all pipelines. 3. Extensibility - The system must allow adding new languages. - The system must allow adding new cognate tiers. - The system must allow adding new frequency lists. 4. Usability - The interface must remain simple and teacher‑focused. - The system must avoid student‑facing complexity.

1. Purpose, Scope, vision and non-goals
WordList Writer is a teacher‑facing authoring tool designed to help instructors create controlled texts for beginner learners. Students never interact with the app directly. Instead, teachers use WordList Writer to craft texts that follow a curriculum, respect frequency constraints, and leverage cognate scaffolding across multiple languages.

The tool currently serves two core instructional functions:

Building a unified beginner‑level master vocabulary list
WordList Writer helps determine whether a single, cross‑linguistic beginner curriculum can be constructed across English, Spanish, Koine Greek, and Latin. The system highlights vocabulary usage, checks frequency alignment, identifies cognates, and surfaces curriculum‑order violations to support the creation of a coherent, multi‑language foundational lexicon.

Supporting multiple master vocabulary lists when needed
If a unified list is not feasible for every situation or student context, WordList Writer allows teachers to create and maintain multiple master vocabulary lists tailored to different learner profiles — such as age group, subject matter, or target language. Each list benefits from the same analysis pipeline: frequency checking, cognate detection, lemma normalization, curriculum‑order validation, and project‑level vocabulary tracking.

WordList Writer’s scope is intentionally focused on teacher workflow, not student interaction. All highlighting, cognate metadata, frequency analysis, and curriculum diagnostics exist to support the teacher’s authoring process. The final output delivered to students is a clean, pedagogically controlled text without any of the analytical overlays.

This PRD defines the functional requirements, constraints, and roadmap for WordList Writer as a professional tool for curriculum‑aligned text development. Future expansions may include enhanced curriculum diagnostics, multi‑list comparison tools, and deeper cross‑language vocabulary modeling, but the core mission remains: to help teachers write texts that are linguistically appropriate, pedagogically sequenced, and cross‑linguistically informed.

Vision Statement WordList Writer aims to become a professional authoring environment for teachers who create controlled texts for beginner learners across multiple languages. The tool provides curriculum‑aligned vocabulary control, frequency awareness, cognate scaffolding, and cross‑language lemma normalization. Its long‑term vision is to support teachers in building coherent beginner‑level curricula—either unified across languages or tailored to specific learner profiles—while maintaining a clean, teacher‑focused workflow that produces pedagogically appropriate texts for students.

1. Purpose and Scope
WordList Writer is a teacher-facing authoring tool for creating controlled beginner-level texts across English, Spanish, Koine Greek, and Latin. Students never interact with the app. Teachers use WordList Writer to craft texts aligned with curriculum order, frequency constraints, cognate scaffolding, and cross-language lemma normalization.

The tool supports two instructional goals:
* Building a unified beginner-level master vocabulary list across languages.
* Supporting multiple master lists when a unified list is not feasible.

The final output delivered to students is a clean text without highlights or diagnostics.

Non-Goals:
* No student accounts or progress tracking
* No quizzes, flashcards, or gamification
* No LMS integration
* No student-facing UI
* No automated grading or AI-generated exercises

2. Core Features

2.1 Writing Window
* Large text input area
* Real-time tokenization
* Lemma normalization per language
* Strict highlighting priority:
  * Green = cognate
  * Black = known (frequency or project list)
  * Red = off-list
* Tooltip shows lemma, cognate tier, frequency rank, curriculum status
* Export clean text (no highlights)

2.2 Top Panel (Four Columns)
Column A: Frequency List
* Displays active frequency list
* Sorted by rank
* Click → highlight in writing window

Column B: Cognates
* English ↔ Spanish/Latin/Greek
* Tiered cognate classification
* Click → highlight in writing window

Column C: Project Word List
* Tracks lemmas appearing in current text
* Normalized before insertion
* Shows cognate tier and frequency tier
* Stored in Supabase

Column D: Master List
* Stable curriculum sequence
* Fields:
  * master_rank
  * english_lemma
  * spanish_lemma
  * latin_root
  * greek_root
  * cognate_flags
  * frequency_ranks
* Highlighting uses Master List membership and rank
* Red = not in Master List or used too early
* Black = allowed
* Green = cognate (overrides)
* Hover shows rank or “Not in Master List”
* Editable: add, insert, reorder, update flags
* Updates highlighting immediately

2.3 Curriculum Violations Panel
Reports:
* Out-of-order vocabulary
* Unknown words
* Curriculum gaps

Behavior:
* Updates automatically
* Never interrupts writing
* Replaces popup warnings
* Scrollable, teacher-facing only

3. Multilingual Capability
Supported languages:
* English
* Spanish
* Koine Greek
* Latin

Static JSON modules:
* frequency/<language>.json
* lemmas/<language>.json
* cognates/<language>.json

Tokenizer rules:
* English/Spanish: whitespace + punctuation
* Greek/Latin: Unicode-aware, accent-aware

4. Highlighting Logic (Priority)
Step 1: Cognate (tier color)
Step 2: Project Word List (black)
Step 3: Frequency List (black)
Step 4: Off-list (red)

Tier colors:
* Latin = gold
* Greek = blue
* Biblical = purple
* General = green

Normalization:
* NFD + accent stripping

5. Word Lists

5.1 Frequency Lists
Requirements:
* NGSL-1K must contain exactly 1000 unique lemmas
* NGSL-Full must contain exactly 2800 lemmas
* Unique lemma requirement after normalization
* Rank continuity required
* Fail-fast ingestion
* Pre-ingestion validation detects:
  * duplicates
  * missing ranks
  * discontinuities
  * malformed entries
  * normalization collisions
  * truncated files

5.2 Lemma Maps
* Inflected → lemma
* Normalized consistently

5.3 Cognate Lists
* English ↔ Spanish/Latin/Greek
* Tier metadata
* Consistent across highlight loop, tooltip, project list

5.4 Project Word Lists (Supabase)
* Stored per project
* Normalized lemmas
* Cognate tier + frequency tier displayed

5.5 Master Lists (Supabase)
Tracks:
* English lemma
* Spanish equivalent
* Latin root
* Greek root
* Cognate flags
* Frequency ranks

Curriculum ordering:
* Master List defines pedagogical sequence
* Insert new words after last used Master List word
* Cognates follow same insertion rule
* Project List does not affect Master List order

Order warning:
* Detects out-of-order usage
* Shows expected lemma and suggested correction

6. UI Container Requirements
Required HTML elements:
* #master-list-container
* #violations-panel
* #top-panel (CSS grid with 4 columns)
* Each column wrapped in .column

Grid:
  #top-panel {
  display: grid;
  grid-template-columns: 180px 180px 180px 1fr;
  }

Placement:
* #violations-panel must be outside #top-panel
* Column D must be fourth child

7. Architecture Overview

Frontend:
* HTML/CSS/JS
* Three top windows + writing window
* Supabase client for storage

Backend (Node.js + Express):
* Serves static frontend
* Serves JSON lists
* API endpoints for project and master list operations

Database (Supabase):
Tables:
* projects
* project_wordlists
* master_wordlists
* cross_language_master

Persistence:
* UUIDs generated client-side
* .select() required for all inserts
* Projects must return full inserted rows

8. Save and Load Pipelines

Save Pipeline:
Step 1: save project metadata
Step 2: save project wordlist
Step 3: save master list updates

Load Pipeline:
Step 1: load project metadata
Step 2: load project wordlist
Step 3: reconstruct UI state

9. Roadmap (Concise)

Phase 1: English-only rebuild
* Tokenizer, lemma map, frequency list, highlighting, project list, violations panel, local master list

Phase 2: Supabase integration
* Projects, project_wordlists, master_wordlists
* Save/load pipeline
* ID propagation

Phase 3: Curriculum modeling
* Out-of-order detection
* Curriculum warnings
* Master list editing
* Cognate insertion rules

Phase 4: Multiple curriculum support
* Multiple master lists
* Comparison tools
* Cross-language master list

Phase 5: Teacher workflow enhancements
* Difficulty metrics
* Coverage metrics
* Export options

Phase 6: Optional enhancements
* Collaboration
* Versioning
* Mobile layout
* PWA

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
