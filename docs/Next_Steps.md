Next Steps — WordList Writer
Version: 2026-07-23
Status: Authoritative Source of Truth

Overview
This document defines the immediate, short-term, medium-term, and long-term development steps for WordList Writer. It reflects the simplified Phase 2 architecture: English-only analysis, simple cognate window, stable tokenizer, stable highlight pipeline, and Supabase-backed persistence. All Phase 3 features (dictionary profiles, alphabetical dictionary, merged dictionary, tier metadata, pending/official cognates, cross-language master) have been removed. This document is restart-proof and reconstructs the entire development sequence.

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

5. Implement Tokenizer + Highlighting Logic (Phase 2)
Step 1: build tokenizeUnified  
Step 2: build normalizeLemma  
Step 3: implement highlight priority:
* green underline = cognate
* normal = known (frequency or master list)
* red asterisk = unknown
Rules:
* tokenizer must not trigger highlight or autosave
* tokenizer must preserve whitespace and punctuation
* highlight must run only through requestHighlightUpdate
Goal:
* English-only highlighting end-to-end

6. Build Four-Column UI (Phase 2)
Column A: frequency list  
Column B: simple cognate window  
Column C: project word list  
Column D: master list  
Goal:
* UI layout matches PRD and updated architecture

7. Prepare for Supabase Integration (Phase 2)
Step 1: create Supabase project  
Step 2: create tables:
* projects
* project_wordlists
* master_wordlists
Step 3: add environment variables to Render  
Rules:
* Supabase v2 requires .select() after inserts
* masterList must contain plain strings only
Goal:
* database ready for Phase 2

8. Begin Phase 1 Development (Completed)
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
* add Spanish frequency list
* add Spanish lemma map
* add Spanish cognates beyond simple list
* add Latin and Greek frequency lists
* add Latin and Greek lemma maps
Update:
* multilingual expansion begins after Phase 2

10. Next Steps After Violations Panel (Phase 2)
* validate violation detection accuracy
* add optional enhancements (sorting, grouping, toggling)
* complete Supabase integration
* update documentation after each feature
Rules:
* violations must run after highlight
* violations must not trigger highlight

11. Completed Work (Phase 2)
* centralized highlight pipeline
* IME-safe input handling
* single-layer editor
* English-only cognate detection
* English-only project list
* English-only master list
* updated startup sequence
* updated save/load pipeline
* stable Supabase integration

12. Upcoming Work (Phase 2)
* tokenizer normalization upgrade
* frequency tier integration
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
Step 3: /api/project/save must accept or generate ID and return it  
Step 4: saveEverything:
* save project first
* capture returned ID
* pass ID to wordlist save
Step 5: /api/project/wordlist/save must receive { projectId, wordlist }

14. Remaining Work After Pipeline Fix (Phase 2)
* verify project switching
* add validation for master list rows
* add UI indicators for save/load success
* add error handling for missing projectId
* add export clean text
* add mobile layout

15. Task Timeline (Phase 2)
Immediate Tasks
* fix highlight substring bug
* validate tokenizer output
* add NAWL support
* add project list editing

Short-Term Tasks
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
This Next Steps document merges the original development plan with updated Phase 2 architecture rules, pipelines, and July 19 fixes. It defines the complete sequence for continuing development and ensures the project remains restart-proof and aligned with the PRD.
