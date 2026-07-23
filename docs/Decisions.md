Decisions — WordList Writer
Version: 2026-07-20
Status: Authoritative Source of Truth

1. Tech Stack Decisions
Node.js + Express
* same language frontend + backend
* fast development
* easy JSON handling
* ideal for lightweight APIs
* works well on Render

Vanilla JavaScript Frontend
* no frameworks
* faster load times
* lower complexity
* easier debugging
* no build tools required

Supabase (PostgreSQL)
* persistent storage for projects and master lists
* built-in REST API
* easy JS client
* real-time updates
* secure row-level policies
* no server maintenance

Static JSON Files
* frequency lists
* lemma maps
* cognate lists
* fast load
* rarely change
* easy version control

API Routes Before Static Middleware
* prevents static fallback from swallowing API requests

Controlled Save Instead of Autosave
* prevents accidental curriculum overwrites
* autosave used only for text, not master list

2. Data Architecture Decisions
Supabase for Project + Master Lists
* user-specific data stored in Supabase
* static linguistic data stored in JSON
* fast startup
* cross-device syncing

Master List Editing in UI
* add lemmas
* insert at specific ranks
* reorder
* update cognate flags
* update frequency metadata
* immediate highlight updates
* Supabase sync in Phase 2

Master List is a Curriculum
* not alphabetical
* not usage-ordered
* pedagogical sequence defined by developer

Cognate Insertion Rule
* insert after last master list lemma appearing in story

Typed Word Insertion Rule
* same insertion rule as cognates

Project List Behavior
* tracks usage only
* does not modify master list

Warning System
* detects curriculum-order violations

Unified Tier-Aware Cognate Highlighting
* single COGNATE_MAP with tier metadata
* global TIER_COLORS and TIER_MAP
* shared across highlight pipeline and project list

Supabase .select() Requirement
* all inserts must use .select() to return inserted rows

Master List Architecture (Updated)
* masterList contains plain strings only
* multilingual fields removed until Phase 3
* English-only structure stable and predictable

Supabase Before Multilingual Expansion
* English-only pipeline stable
* multilingual support requires persistent storage
* Supabase provides foundation for Phase 3
Updated dependency chain:
* English-only pipeline (complete)
* Supabase integration (current)
* Multilingual support (next)
* UI polish + deployment

3. UI/UX Decisions
Four-Column Top Panel (A, B, C, D)
* mirrors teacher mental model
* keeps reference lists visible
* reduces clicks
* supports fast writing flow

Bottom Writing Window
* single contenteditable editor
* large, distraction-free
* real-time highlighting requires clarity

Highlighting Priority
* cognate (green underline)
* known (normal)
* unknown (red asterisk)

Master List Column (D)
* displays curated beginner vocabulary sequence
* supports editing
* highlighting uses master list membership
* red = not in list or too early

Cognate Click Behavior
* highlights matching tokens
* inserts cognate into master list

Project List Cognate Priority
* cognates appear at top of Column C with tier badge

Auto-Scroll for Master List
* deferred until multilingual master list is stable

Spanish as First Multilingual Target
* clean frequency data
* reliable lemma resources
* strong cognate overlap

Frequency List Format
* uniform structure:
  * rank
  * lemma

Lemma Map Format
* flat inflected → lemma mapping
* no POS tags
* no metadata
* no nested objects

Highlight Logic Extension
* language-aware
* priority preserved:
  * cognate → known → unknown

Replace Popup with Violations Panel
* popups interrupt writing
* panel provides stable diagnostics
* supports sorting, grouping, toggling

4. Frontend Architecture Decisions (July 19)
Single-Layer Editor
* no overlay
* no nested spans
* editor.innerHTML replaced on each highlight pass

Centralized Highlight Pipeline
* all highlight operations must enter through requestHighlightUpdate
* requestHighlightUpdate uses debounce
* no highlight during IME composition
* handleStableInput must never be called directly

Tokenizer Rules
* tokenizeUnified must not trigger highlight or autosave
* punctuation and whitespace preserved
* normalized tokens not logged

Startup Sequence
Step 1: load language and cognates
Step 2: load project text
Step 3: load master list
Step 4: load project wordlist
Step 5: load violations
Step 6: trigger highlight once
Step 7: after 75ms run project list and order check
Step 8: display "Load complete"

Frequency Known-Word Rules
* known words come from NGSL-1K
* frequencySet contains normalized lemmas

Autosave Rules
* autosave must be debounced
* autosave pauses during project resets

5. Backend Architecture Decisions
Project ID Rules
* project-id-input must update on create, load, save, and new project
* incorrect project ID causes empty master list loads

Master List Rules
* masterList must contain plain strings only
* Supabase rejects rows where lemma is undefined

Static Middleware Rule
* API routes must be registered before express.static()

SaveEverything Contract
* save project first
* capture returned ID
* save project wordlist
* save master list

6. Multilingual Decisions
Supported Languages
* English
* Spanish
* Koine Greek
* Latin

Dynamic Language Modules
* load frequency, lemma, cognate files based on selected language
* reduces memory usage
* faster startup
* cleaner architecture

7. Deployment Decisions
Render Hosting
* free tier
* GitHub auto-deploy
* easy environment variables

Supabase Hosting
* persistent data
* zero maintenance
* PostgreSQL reliability

8. Repository Structure Decisions
Use /docs Folder
* organized repo
* permanent memory
* industry standard

Clean Rebuild in New Repo
* avoid legacy clutter
* remove Ruby artifacts
* clean architecture

9. Future Considerations
Mobile Support (Deferred)
* responsive layout
* collapsible panels
* touch interactions
* local caching
* optional PWA wrapper

Language Visibility Toggles (Deferred)
* hide/show language columns in master list
* prevent UI clutter

Additional Future Options
* cloud sync
* Kindle-ready export
* image integration
* animation script export
* collaboration mode
* advanced stats

10. Debugging Decisions (Historical)
Frontend Fixes (2026-07-14)
* fixed missing brace in renderViolationsPanel
* repaired template string in renderFrequencyStats
* removed nested duplicate renderFrequencyStats
* verified global renderer functions

Backend Save/Load Fix (2026-07-15)
* aligned frontend object shape with Supabase schema
* mapped word → lemma on save
* mapped lemma → word on load
* added safety guard to normalizeLemma
* confirmed correct API routes:
  * /api/master/save/:id
  * /api/master/load/:id

Documentation Formatting Reminder
* use plain text section titles
* use asterisks (*) for bullet points
* do not insert blank lines inside bullet lists
* use ASCII-only characters
* avoid Markdown headings (#)
* avoid fenced code blocks unless necessary
* use Step format for workflows to prevent GitHub auto-renumbering
