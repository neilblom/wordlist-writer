# 📄 **PRD.md — WordList Writer (Node.js + Express + Supabase)**

PRD Introduction
WordList Writer is a teacher‑facing authoring tool for creating controlled texts for beginner learners across multiple languages. It provides instructors with real‑time vocabulary analysis, curriculum alignment feedback, cognate scaffolding, and frequency‑based highlighting. Students never interact with the app directly; instead, teachers use WordList Writer to craft texts that are pedagogically sequenced, linguistically appropriate, and aligned with a chosen curriculum or master vocabulary list.

The tool supports two major instructional goals. First, it helps determine whether a unified beginner‑level master vocabulary list can be built across English, Spanish, Koine Greek, and Latin. Second, if a unified list is not feasible, WordList Writer enables teachers to create multiple master vocabulary lists tailored to different learner profiles—such as age group, subject matter, or target language—while maintaining consistent analysis and feedback across all lists.

WordList Writer is designed to be flexible, extensible, and grounded in linguistic accuracy. Its normalization pipeline ensures consistent lemma handling across languages, while its cognate and frequency systems provide teachers with actionable insights during text creation. The final output delivered to students is a clean, controlled text without any analytical overlays.

PRD Requirements
  Functional Requirements
    1. Text Analysis
    - The system must tokenize input text and normalize lemmas across supported languages.
    - The system must highlight words according to curriculum status (known, unknown, out‑of‑order).
    - The system must detect cognates using a tier‑based cognate map.
    - The system must detect frequency tier (NGSL, NAWL, or other lists).
    - The system must display tooltips with lemma, cognate tier, frequency rank, and curriculum status.
    2. Master Vocabulary Lists
    - The system must load a master vocabulary list from local or cloud storage.
    - The system must support multiple master lists (age‑based, subject‑based, language‑based).
    - The system must allow switching between master lists without reloading the app.
    - The system must validate text against the active master list.
    - The system must detect curriculum‑order violations.
    3. Cognate System
    - The system must load cognate lists for English‑Spanish, English‑Latin, and English‑Greek.
    - The system must support tiered cognate classification (general, latin, greek, biblical).
    - The system must allow expansion of cognate lists over time.
    - The system must apply cognate detection consistently across highlight loop, tooltip, and project list.
    4. Frequency System
    - The system must load frequency lists (NGSL, NAWL, or custom lists).
    - The system must detect frequency tier for each lemma.
    - The system must display frequency information in tooltips and project list.
    5. Project List
    - The system must track all lemmas appearing in the current text.
    - The system must normalize lemmas before adding them to the project list.
    - The system must display cognate tier and frequency tier for each lemma.
    - The system must update dynamically as the teacher edits the text.
    6. Teacher Workflow
    - The system must provide a clean, teacher‑facing UI.
    - The system must allow exporting a clean version of the text (no highlights).
    - The system must provide curriculum diagnostics (unknown words, out‑of‑order words).
    - The system must provide a text difficulty summary (planned for later phases).
    - The system must support multi‑language authoring.

  Non‑Functional Requirements
    1. Performance
    - Text analysis must occur in real time as the teacher types.
    - Highlighting must update without noticeable delay.
    2. Reliability
    - The system must handle large texts without crashing.
    - The system must maintain consistent normalization across all pipelines.
    3. Extensibility
    - The system must allow adding new languages.
    - The system must allow adding new cognate tiers.
    - The system must allow adding new frequency lists.
    4. Usability
    - The interface must remain simple and teacher‑focused.
    - The system must avoid student‑facing complexity.

## 1. Purpose, Scope, vision and non-goals

WordList Writer is a teacher‑facing authoring tool designed to help instructors create controlled texts for beginner learners. Students never interact with the app directly. Instead, teachers use WordList Writer to craft texts that follow a curriculum, respect frequency constraints, and leverage cognate scaffolding across multiple languages.

The tool currently serves two core instructional functions:
- Building a unified beginner‑level master vocabulary list  
WordList Writer helps determine whether a single, cross‑linguistic beginner curriculum can be constructed across English, Spanish, Koine Greek, and Latin. The system highlights vocabulary usage, checks frequency alignment, identifies cognates, and surfaces curriculum‑order violations to support the creation of a coherent, multi‑language foundational lexicon.

- Supporting multiple master vocabulary lists when needed  
If a unified list is not feasible for every situation or student context, WordList Writer allows teachers to create and maintain multiple master vocabulary lists tailored to different learner profiles — such as age group, subject matter, or target language. Each list benefits from the same analysis pipeline: frequency checking, cognate detection, lemma normalization, curriculum‑order validation, and project‑level vocabulary tracking.

WordList Writer’s scope is intentionally focused on teacher workflow, not student interaction. All highlighting, cognate metadata, frequency analysis, and curriculum diagnostics exist to support the teacher’s authoring process. The final output delivered to students is a clean, pedagogically controlled text without any of the analytical overlays.

This PRD defines the functional requirements, constraints, and roadmap for WordList Writer as a professional tool for curriculum‑aligned text development. Future expansions may include enhanced curriculum diagnostics, multi‑list comparison tools, and deeper cross‑language vocabulary modeling, but the core mission remains: to help teachers write texts that are linguistically appropriate, pedagogically sequenced, and cross‑linguistically informed.

Vision Statement
WordList Writer aims to become a professional authoring environment for teachers who create controlled texts for beginner learners across multiple languages. The tool provides curriculum‑aligned vocabulary control, frequency awareness, cognate scaffolding, and cross‑language lemma normalization. Its long‑term vision is to support teachers in building coherent beginner‑level curricula—either unified across languages or tailored to specific learner profiles—while maintaining a clean, teacher‑focused workflow that produces pedagogically appropriate texts for students.

Non‑Goals
To maintain a clear scope, WordList Writer explicitly excludes the following:
- Student accounts, logins, or profiles
- Student progress tracking or analytics
- Flashcards, quizzes, or spaced‑repetition systems
- Gamification or reward systems
- Student‑facing UI or interactive learning tools
- Classroom management features
- LMS integration (Canvas, Moodle, Google Classroom, etc.)
- Automated grading or assessment
- AI‑generated student exercises or homework

WordList Writer is not a student learning platform.
It is a teacher‑facing authoring tool for curriculum‑aligned text creation.

---

## 2. Core Features

### 2.1 Writing Window (Bottom Panel)  
- Large text input area  
- Real‑time tokenization  
- Lemmatization based on selected language  
- Highlighting rules (strict priority):  
  - **Green** = cognate  
  - **Black/normal** = in project list or frequency list  
  - **Red** = off‑list
 
Section: Save + Backup System
- Controlled save button in UI
- POST /api/save-master-list
- Writes to public/master/master_list.json
- Auto‑creates public/master/backups/
- Timestamped backup naming: master_list_YYYY-MM-DDTHH-MM-SS.json
- Save button shows status messages (“Saving…”, “Saved”, “Failed”)

### Requirement: Master List Simplicity
The master list must remain English-only until the core pipeline is fully stable and Supabase integration is complete.

### 2.2 Top Panel (Three Columns)

#### Column A — Frequency List  
- Displays active frequency list  
- Sorted by rank  
- Click → highlight in writing window  

#### Column B — Cognate Window  
- Displays cognates relevant to selected language  
- Supports: English ↔ Spanish/Latin/Greek  
- Click → highlight in writing window  

#### Column C — Project Word List  
- Custom list per project  
- User‑editable  
- Stored in Supabase  

Master List (Column D)
The Master List is a stable, curated vocabulary sequence used across all languages supported by the app. It defines the beginner‑level lemmas (initially ~400) and their equivalents in English, Spanish, Latin, and Greek.

Master List Fields
master_rank (1–400)

english_lemma

spanish_lemma

latin_root

greek_root

cognate_flags

frequency_ranks (per language)

Behavior
The Master List is displayed in Column D.

Highlighting in the writing window uses Master List membership and rank.

Red = not in Master List or used too early.

Black = allowed (in Master List or Project List).

Green = cognate (overrides other colors).

Hovering shows Master List rank or “Not in Master List.”

---
Tier‑Aware Highlighting (Current Version)
The editor highlights English words based on cognate tier metadata.

Highlight colors:

Latin → gold

Greek → blue

Biblical → purple

General → green

Highlight rules:

If a word’s normalized lemma exists in COGNATE_MAP, highlight it using its tier color.

If the lemma exists in the Master List, render normally.

If the lemma is unknown and not a function word, append a red asterisk.

Normalization:

All cognate lookups use NFD normalization and accent‑stripping.

Tooltip:

  Shows cognate target (Spanish) and tier metadata.

Master List Ordering (Curriculum Mode)
The Master List represents a global curriculum of English lemmas.
It is not ordered alphabetically and not ordered by story usage.
It is ordered according to a pedagogical sequence defined by the developer.

The Master List is loaded from JSON in the exact order defined by the curriculum.

New words (typed or clicked) are inserted after the last Master List word that appears in the story, using lemma matching.

Cognates added from the Cognate Window follow the same insertion rule.

Manual reordering is allowed via the Edit UI.

The Project List does not affect Master List order.

This ensures the curriculum grows naturally while preserving the intended learning sequence.

Order Warning System
When writing a story, the system checks whether the text follows the curriculum order.

The app identifies the next expected Master List word that has not yet appeared.

If the story uses a word whose Master List rank is greater than the next expected word, a warning is shown.

The warning appears in the floating tooltip and identifies:

the out‑of‑order lemma

the expected lemma

the suggested correction (adjust curriculum or revise text)

Editing
The user may modify the Master List directly inside Column D:

add new lemmas

insert lemmas at specific ranks

reorder lemmas

add cross‑language equivalents

update cognate flags

Changes update highlighting immediately.
---

### Master List — Language Visibility Controls (Future)

The Master List will include UI controls allowing users to toggle visibility of
language columns. This supports multilingual vocabulary development while keeping
the interface clean.

Requirements:
- Preserve rank-based ordering
- Dynamically hide/show columns without reload
- Support arbitrary languages in JSON

Non-Goals:
- No sorting changes
- No automatic language detection

## 3. Multilingual Capability

### Supported Languages  
- English  
- Spanish  
- Koine Greek  
- Latin  

### Language Modules (Static JSON)  
- `frequency/<language>.json`  
- `lemmas/<language>.json`  
- `cognates/<language>.json`  

### Tokenizer Rules  
- English/Spanish: whitespace + punctuation  
- Greek/Latin: Unicode‑aware, accent‑aware  

---

## 4. Highlighting Logic (Priority Order)

1. **Cognate (Green)**  
2. **Project Word List (Black)**  
3. **Frequency List (Black)**  
4. **Off‑List (Red)**  

---

## 5. Word Lists

### 5.1 Frequency Lists (Static JSON)

#### ⭐ English Frequency List Source (NGSL)  
The app uses the **public‑domain NGSL** from GitHub:

`https://github.com/neilblom/wordlist-writer/tree/main/ngsl`

Stored as:

```
frequency/english_ngsl.json
```

### 5.2 Lemma Maps  
Maps inflected forms → lemma.

### 5.3 Cognate Lists  
English ↔ Spanish/Latin/Greek.

### 5.4 Project Word Lists (Supabase)  
Stored per project.

### 5.5 Master Lists (Supabase)

#### A. Master List per Language  
Tracks all words used across all projects.

#### B. Cross‑Language Master List  
Stores:

- English lemma  
- Spanish equivalent  
- Latin root  
- Greek root  
- Cognate flags  
- Frequency ranks  

---

## 6. Architecture Overview

### Frontend  
- HTML/CSS/JS  
- Three top windows + bottom writing window  
- Supabase client for project storage  

### Backend — Node.js + Express  
- Serves static frontend  
- Serves static JSON lists  
- API endpoints for project/master list operations  

### Database — Supabase  
Tables:

- `projects`  
- `project_wordlists`  
- `master_wordlists`  
- `cross_language_master`  

---

## 7. Roadmap  
### Phase 5 — Deployment (Render)
