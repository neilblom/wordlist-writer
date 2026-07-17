Next Steps — WordList Writer
Version: 2026-07-17
Status: Authoritative Source of Truth

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
* Clean separation of backend, frontend, and static data
* Predictable navigation and maintainability

2. Initialize Node.js + Express
Step 1: npm init -y  
Step 2: Install Express  
Step 3: Create src/server.js  
Step 4: Configure Express to:
* Serve static files from /public
* Serve JSON lists from /frequency, /lemmas, /cognates
* Provide basic health-check route

Goal:
* Local server running index.html

3. Add English Frequency List (NGSL)
Step 1: Create frequency/english_ngsl.json  
Step 2: Add NGSL data  
Step 3: Load via fetch() in frontend

Goal:
* Frequency list appears in Column A

4. Add English Lemma Map
Step 1: Create lemmas/english.json  
Step 2: Add lemma mappings  
Step 3: Load in frontend

Goal:
* Lemma resolution works in writing window

5. Implement Tokenizer + Highlighting Logic
Step 1: Build tokenizer in app.js  
Step 2: Build lemma lookup  
Step 3: Implement highlight priority:
* Green = cognate
* Black = frequency or project list
* Red = unknown

Goal:
* English-only highlighting end-to-end

6. Build Three-Column UI (Phase 1)
Column A: Frequency list  
Column B: Cognates (placeholder)  
Column C: Project word list  

Goal:
* UI layout matches PRD

7. Prepare for Supabase Integration
Step 1: Create Supabase project  
Step 2: Create tables:
* projects
* project_wordlists
* master_wordlists
* cross_language_master
Step 3: Add environment variables to Render (later)

Goal:
* Database ready for Phase 2

8. Begin Phase 1 Development
Deliverable:
* Writing window
* Frequency list
* Lemma lookup
* Highlighting
* Basic UI

Purpose:
* Foundation for all future phases

9. Multilingual Expansion (Deferred)
* Add Spanish cognates
* Add Latin cognates
* Add Greek cognates
* Extend tokenizer and lemmaMap
* Revisit Master List structure after Supabase integration

Update:
* Supabase required before multilingual support
* Multilingual expansion begins after Phase 2

10. Next Steps After Violations Panel
* Validate violation detection accuracy
* Add optional enhancements (sorting, grouping, toggling)
* Begin Phase 2 (Supabase integration)
* Update documentation after each feature

11. Completed Work
* Tier-aware highlighting
* Tier-aware tooltip
* Tier-aware project list
* Unified cognate lookup map
* Global tier metadata

12. Upcoming Work
* Tokenizer normalization upgrade
* Tier filtering UI
* Frequency tier integration
* Multi-word cognate support (paused)

13. Backend Save Pipeline Issues (Historical)
Errors:
* null value in column "id"
* Missing projectId

Root Causes:
* currentProjectId null during save
* Wordlist save runs before project save
* Backend not returning ID
* Frontend not propagating ID

Required Fixes:
Step 1: new-project-btn sets currentProjectId = crypto.randomUUID()  
Step 2: load-project-btn sets currentProjectId = projectId  
Step 3: /api/projects/save must accept or generate ID and return it  
Step 4: saveEverything():
* Save project first
* Capture returned ID
* Pass ID to wordlist save
Step 5: /api/projects/wordlist/save must receive { projectId, wordlist }

14. Remaining Work After Pipeline Fix (2026-07-15)
* Verify project switching
* Add multilingual lemma mapping
* Add validation for Master List rows
* Add UI indicators for save/load success
* Add error handling for missing projectId

15. Task Timeline
Immediate Tasks
* Fix highlight substring bug
* Validate tokenizer output
* Add NAWL / BSL / TSL support
* Add cognate editing UI

Short-Term Tasks
* Add project list editing
* Add Supabase save/load
* Add export features
* Add mobile layout

Medium-Term Tasks
* Add teacher tools
* Add vocabulary export
* Add quiz generator

Long-Term Tasks
* Add multi-language support
* Add AI-assisted text simplification
* Add classroom analytics

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
