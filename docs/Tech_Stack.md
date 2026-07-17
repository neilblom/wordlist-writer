Tech Stack — WordList Writer
Version: 2026-07-17
Status: Authoritative Source of Truth

1. Frontend Stack
HTML5
* Three-column top panel
* Bottom writing window
* Language selector
* Word list displays

CSS3
* Clean, minimal UI
* Three-column responsive layout
* Highlight colors (green, black, red)
* Future dark-mode support

Vanilla JavaScript (ES6+)
* Real-time tokenization
* Lemma lookup
* Highlighting logic
* Load static JSON lists
* Update project word lists
* Communicate with Supabase
* No frameworks (React/Vue/etc.)

Tier-Aware Cognate Architecture
Global structures:
* TIER_COLORS (tier → color)
* TIER_MAP (lemma → tier)
* COGNATE_MAP (unified lookup)

Population:
* Build COGNATE_MAP at startup from cognate JSON files
* COGNATE_MAP[key] = { es, tier: TIER_MAP[key] || "general" }

Consumers:
* renderHighlights
* renderProjectList
* showMasterTooltip

Normalization:
* lemma.toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g, "")

2. Backend Stack
Node.js (LTS)
* Same language frontend + backend
* Easy JSON handling
* Fast development cycle
* Ideal for lightweight APIs
* Works well on Render

Express.js
* Serve static frontend
* Serve static JSON lists (frequency, lemmas, cognates)
* REST API endpoints:
  * Load/save projects
  * Update master lists
  * Fetch language modules

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
* Static middleware: app.use(express.static(path.join(__dirname, "public")))
* API routes must be registered before static middleware
* JSON body parsing: app.use(express.json())
* File writes via Node fs
* Relative paths based on __dirname

3. Database Stack
Supabase (PostgreSQL)
Tables:
* projects
* project_wordlists
* master_wordlists
* cross_language_master

Reasons for Supabase:
* Generous free tier
* Built-in REST API
* Easy JS client
* Real-time updates
* Secure row-level policies
* No server maintenance

Supabase is source of truth for:
* Project text
* Project word lists
* Master vocabulary tracking
* Cross-language relationships

Supabase v2:
* Returns minimal by default
* .select() required to retrieve inserted rows

4. Static Data Files
Stored in repo and served by Express.

Frequency Lists
frequency/english_ngsl.json
frequency/spanish.json
frequency/greek.json
frequency/latin.json

Lemma Maps
lemmas/english.json
lemmas/spanish.json
lemmas/greek.json
lemmas/latin.json

Cognate Lists
cognates/english_spanish.json
cognates/english_latin.json
cognates/english_greek.json

Static JSON rationale:
* Instant load
* No DB queries
* Rarely change
* Backend stays simple

5. Hosting and Deployment
Render (Backend Hosting)
* Free tier
* GitHub integration
* Automatic redeploys
* Environment variable support
* Works well with Supabase

Supabase (Database Hosting)
* Persistent storage
* Secure policies
* Easy JS client

GitHub (Source Control)
* Source code
* Static JSON lists
* Documentation (/docs)
* Project history

6. Development Tools
VS Code
* Recommended editor

Git + GitHub
* Version control
* Collaboration

Node Version Manager (nvm)
* Optional
* Manage Node versions

7. Backend → Supabase Integration
Master List Save Route
POST /api/master/save/:projectId
* Deletes existing rows for project
* Inserts new rows using:
  * word → lemma
  * rank → rank
  * language → language
  * projectId → project_id

Master List Load Route
GET /api/master/load/:projectId
* Returns rows where project_id = :id
* Frontend converts lemma → word before rendering

Notes
* Supabase rejects rows where lemma is undefined
* Frontend must always send valid word or english

8. Summary
WordList Writer tech stack:
* Frontend: HTML, CSS, Vanilla JS
* Backend: Node.js + Express
* Database: Supabase (PostgreSQL)
* Static Data: JSON files
* Hosting: Render + Supabase
* Source Control: GitHub

Benefits:
* Fast development
* Easy debugging
* Low hosting cost
* Long-term stability
* No framework lock-in

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
