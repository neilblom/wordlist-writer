Tech Stack — WordList Writer
Version: 2026-07-20
Status: Authoritative Source of Truth

1. Frontend Stack
HTML5
* single contenteditable writing window
* four-column top panel
* violations panel
* language selector
* project and master list displays

CSS3
* clean, minimal UI
* responsive four-column layout
* highlight colors (green underline, red asterisk, normal known)
* future dark-mode support

Vanilla JavaScript (ES6+)
* centralized highlight pipeline
* IME-safe input handling
* real-time tokenization
* lemma normalization
* cognate detection
* frequency detection
* curriculum-order detection
* project list and master list rendering
* autosave debounce logic
* Supabase communication
* no frameworks (React/Vue/etc.)

Tier-Aware Cognate Architecture
Global structures:
* TIER_COLORS (tier → color)
* TIER_MAP (lemma → tier)
* COGNATE_MAP (unified lookup)
Population:
* build COGNATE_MAP at startup from cognate JSON files
* COGNATE_MAP[key] = { es, tier: TIER_MAP[key] || "general" }
Consumers:
* renderHighlightsFast
* renderProjectList
Normalization:
* lemma.toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g, "")

2. Backend Stack
Node.js (LTS)
* same language frontend + backend
* easy JSON handling
* fast development cycle
* ideal for lightweight APIs
* works well on Render

Express.js
* serve static frontend
* serve static JSON lists (frequency, lemmas, cognates)
* REST API endpoints:
  * save project
  * load project
  * save master list
  * load master list
Backend File Structure
src/
server.js
routes/
controllers/
utils/
public/
frequency/
lemmas/
cognates/
Express Server Structure
* server.js in project root
* static middleware: app.use(express.static(path.join(__dirname, "public")))
* API routes registered before static middleware
* JSON body parsing: app.use(express.json())
* file writes via Node fs
* relative paths based on __dirname

3. Database Stack
Supabase (PostgreSQL)
Tables:
* projects
* project_wordlists
* master_wordlists
* cross_language_master
Reasons for Supabase:
* generous free tier
* built-in REST API
* easy JS client
* real-time updates
* secure row-level policies
* no server maintenance
Supabase is source of truth for:
* project text
* project word lists
* master vocabulary tracking
* cross-language relationships
Supabase v2:
* returns minimal by default
* .select() required to retrieve inserted rows

4. Static Data Files
Stored in repo and served by Express.
Frequency Lists
* frequency/english_ngsl.json
* frequency/spanish.json
* frequency/greek.json
* frequency/latin.json
Lemma Maps
* lemmas/english.json
* lemmas/spanish.json
* lemmas/greek.json
* lemmas/latin.json
Cognate Lists
* cognates/english_spanish.json
* cognates/english_latin.json
* cognates/english_greek.json
Static JSON rationale:
* instant load
* no DB queries
* rarely change
* backend stays simple

5. Hosting and Deployment
Render (Backend Hosting)
* free tier
* GitHub integration
* automatic redeploys
* environment variable support
* works well with Supabase
Supabase (Database Hosting)
* persistent storage
* secure policies
* easy JS client
GitHub (Source Control)
* source code
* static JSON lists
* documentation (/docs)
* project history

6. Development Tools
VS Code
* recommended editor
Git + GitHub
* version control
* collaboration
Node Version Manager (nvm)
* optional
* manage Node versions

7. Backend → Supabase Integration
Master List Save Route
POST /api/master/save/:projectId
* deletes existing rows for project
* inserts new rows using:
  * lemma → lemma
  * projectId → project_id
Master List Load Route
GET /api/master/load/:projectId
* returns rows where project_id = :id
* frontend assigns masterList directly from returned data
Notes
* Supabase rejects rows where lemma is undefined
* masterList must contain plain strings only
* project-id-input must always be updated before save/load

8. Updated Architecture Rules
Highlight Pipeline
* all highlight operations must enter through requestHighlightUpdate
* requestHighlightUpdate uses debounce
* no highlight during IME composition
Tokenizer Rules
* tokenizeUnified must not trigger highlight or autosave
* punctuation and whitespace remain raw strings
Rendering Rules
* single-layer editor
* no overlay
* no nested spans
* editor.innerHTML replaced on each highlight pass
Frequency Rules
* known words come from NGSL-1K
* frequencySet contains normalized lemmas
Autosave Rules
* autosave must be debounced
* autosave pauses during project resets
Startup Sequence
Step 1: load language and cognates
Step 2: load project text
Step 3: load master list
Step 4: load project wordlist
Step 5: load violations
Step 6: trigger highlight once
Step 7: after 75ms run project list and order check
Step 8: display "Load complete"

9. Summary
WordList Writer tech stack:
* frontend: HTML, CSS, Vanilla JS
* backend: Node.js + Express
* database: Supabase (PostgreSQL)
* static data: JSON files
* hosting: Render + Supabase
* source control: GitHub
Benefits:
* fast development
* easy debugging
* low hosting cost
* long-term stability
* no framework lock-in
