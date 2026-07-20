Next Steps — WordList Writer
Version: 2026-07-20
Status: Authoritative Source of Truth

Overview
This document defines the immediate, short-term, medium-term, and long-term development steps for WordList Writer. It merges the original plan with updated July 19 architecture rules, tokenizer rules, highlight pipeline rules, rendering rules, Supabase integration rules, dictionary profile system, hybrid cognate window, alphabetical dictionary, cognate merging rules, and project ID fixes. It is restart-proof and reconstructs the entire development sequence.

1. Set Up Project Structure
Create base folder layout:
* docs
* public
  * cognates
  * frequency
  * lemmas
  * master
    * backups
* src
  * controllers
  * lib
  * routes
  * utils
* scripts
Purpose:
* clean separation of backend, frontend, and static data
* predictable navigation and maintainability

2. Initialize Node.js + Express
Step 1: npm init -y  
Step 2: install Express  
Step 3: create src/server.js  
Step 4: configure Express to:
* serve static files from /public
* serve JSON lists from /frequency, /lemmas, /cognates
* provide basic health-check route
Goal:
* local server running index.html

3. Add English Frequency List (NGSL)
Step 1: create frequency/english_ngsl.json  
Step 2: add NGSL data  
Step 3: load via fetch in frontend  
Goal:
* frequency list appears in Column A

4. Add English Lemma Map
Step 1: create lemmas/english.json  
Step 2: add lemma mappings  
Step 3: load in frontend  
Goal:
* lemma resolution works in writing window

5. Implement Tokenizer + Highlighting Logic (Updated)
Step 1: build tokenizeUnified  
Step 2: build normalizeLemma  
Step 3: implement highlight priority:
* green underline = cognate
* normal = known (frequency or master list)
* red asterisk = unknown
Updated Rules:
* tokenizer must not trigger highlight or autosave
* tokenizer must preserve whitespace and punctuation
* highlight must run only through requestHighlightUpdate
Goal:
* English-only highlighting end-to-end

6. Build Four-Column UI (Updated)
Column A: frequency list  
Column B: cognates (hybrid window)  
Column C: project word list  
Column D: master list  
Goal:
* UI layout matches PRD and updated architecture

7. Prepare for Supabase Integration (Updated)
Step 1: create Supabase project  
Step 2: create tables:
* projects
* project_wordlists
* master_wordlists
* cognates_official
* cognates_pending
* dictionary_profiles
* cross_language_master
Step 3: add environment variables to Render  
Updated Rules:
* Supabase v2 requires .select() after inserts
* masterList must contain plain strings only
Goal:
* database ready for Phase 2

8. Begin Phase 1 Development (Updated)
Deliverable:
* single-layer writing window
* frequency list
* lemma lookup
* centralized highlight pipeline
* IME-safe input handling
* basic UI
Purpose:
* foundation for all future phases

9. Multilingual Expansion (Deferred)
* add Spanish cognates
* add Latin cognates
* add Greek cognates
* extend tokenizer and lemmaMap
* revisit master list structure after Supabase integration
Update:
* multilingual expansion begins after Phase 2

10. Next Steps After Violations Panel (Updated)
* validate violation detection accuracy
* add optional enhancements (sorting, grouping, toggling)
* begin Phase 2 (Supabase integration)
* update documentation after each feature
Updated Rules:
* violations must run after highlight
* violations must not trigger highlight

11. Completed Work (Updated)
* centralized highlight pipeline
* IME-safe input handling
* single-layer editor
* tier-aware highlighting
* tier-aware project list
* unified cognate lookup map
* merged cognate dictionary
* dictionary profile system
* alphabetical cognate dictionary
* pending → official cognate merging
* updated startup sequence
* updated save/load pipeline

12. Upcoming Work (Updated)
* tokenizer normalization upgrade
* tier filtering UI
* frequency tier integration
* multi-word cognate support (paused)
* project list editing
* Supabase save/load UI indicators
* export clean text
* mobile layout

13. Backend Save Pipeline Issues (Historical + Fixed)
Errors:
* null value in column "id"
* missing projectId
Root Causes:
* currentProjectId null during save
* wordlist save ran before project save
* backend not returning ID
* frontend not propagating ID
Required Fixes:
Step 1: new-project-btn sets currentProjectId = crypto.randomUUID()  
Step 2: load-project-btn sets currentProjectId = projectId  
Step 3: /api/projects/save must accept or generate ID and return it  
Step 4: saveEverything:
* save project first
* capture returned ID
* pass ID to wordlist save
Step 5: /api/projects/wordlist/save must receive { projectId, wordlist }

14. Remaining Work After Pipeline Fix (Updated)
* verify project switching
* add multilingual lemma mapping
* add validation for master list rows
* add UI indicators for save/load success
* add error handling for missing projectId
* add dictionary profile save/load UI
* add cognate editing UI
* add publish cognates UI

15. Task Timeline (Updated)
Immediate Tasks
* fix highlight substring bug
* validate tokenizer output
* add NAWL / BSL / TSL support
* add cognate editing UI

Short-Term Tasks
* add project list editing
* add Supabase save/load UI indicators
* add export features
* add mobile layout

Medium-Term Tasks
* add teacher tools
* add vocabulary export
* add quiz generator

Long-Term Tasks
* add multi-language support
* add AI-assisted text simplification
* add classroom analytics

Summary
This Next Steps document merges the original development plan with updated architecture rules, pipelines, dictionary profile system, hybrid cognate window, alphabetical dictionary, cognate merging rules, and July 19 fixes. It defines the complete sequence for continuing development and ensures the project remains restart-proof and aligned with the PRD.

Documentation Formatting Reminder
* use plain text section titles
* use asterisks (*) for bullet points
* do not insert blank lines inside bullet lists
* use ASCII-only characters
* avoid Markdown headings (#)
* avoid fenced code blocks unless necessary
* use Step format for workflows to prevent GitHub auto-renumbering
