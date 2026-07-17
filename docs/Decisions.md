Decisions — WordList Writer
Version: 2026-07-17
Status: Authoritative Source of Truth

1. Tech Stack Decisions
Node.js + Express
* Same language frontend + backend
* Fast development
* Easy JSON handling
* Ideal for lightweight APIs
* Works well on Render

Vanilla JavaScript Frontend
* No frameworks (React/Vue/etc.)
* Faster load times
* Lower complexity
* Easier debugging
* No build tools required

Supabase (PostgreSQL)
* Persistent storage for projects and master lists
* Built-in REST API
* Easy JS client
* Real-time updates
* Secure row-level policies
* No server maintenance

Static JSON Files
* Frequency lists
* Lemma maps
* Cognate lists
* Fast load
* Rarely change
* Easy version control

API Routes Before Static Middleware
* Prevents static fallback from swallowing API requests

Controlled Save Instead of Autosave
* Prevents accidental curriculum overwrites

2. Data Architecture Decisions
Supabase for Project + Master Lists
* User-specific data stored in Supabase
* Static linguistic data stored in JSON
* Fast startup
* Cross-device syncing

Master List Editing in UI
* Add lemmas
* Insert at specific ranks
* Reorder
* Add cross-language equivalents
* Update cognate flags
* Update frequency metadata
* Immediate highlight + tooltip updates
* Supabase sync in Phase 4

Master List is a Curriculum
* Not alphabetical
* Not usage-ordered
* Pedagogical sequence defined by developer

Cognate Insertion Rule
* Insert after last Master List lemma appearing in story

Typed Word Insertion Rule
* Same insertion rule as cognates

Project List Behavior
* Tracks usage only
* Does not modify Master List

Warning System
* Detects curriculum-order violations

Unified Tier-Aware Cognate Highlighting
* Single COGNATE_MAP with tier metadata
* Global TIER_COLORS and TIER_MAP
* Shared across highlight loop, tooltip, project list

Supabase .select() Requirement
* All inserts must use .select() to return inserted rows

Master List Architecture (July 2026)
* English-only until Supabase integration is complete
* Multilingual fields caused inconsistent shapes and crashes
* English-only structure is stable and predictable

Supabase Before Multilingual Expansion
* English-only pipeline stable
* Multilingual support requires persistent storage
* Supabase provides foundation for Phase 3
* Updated dependency chain:
  * English-only pipeline (complete)
  * Supabase integration (current)
  * Multilingual support (next)
  * UI polish + deployment

3. UI/UX Decisions
Three-Column Top Panel (A, B, C)
* Mirrors teacher mental model
* Keeps reference lists visible
* Reduces clicks
* Supports fast writing flow

Bottom Writing Window
* Large, distraction-free
* Real-time highlighting requires clarity

Highlighting Priority
* Cognate (green)
* Known (black)
* Unknown (red)

Add Column D (Master List)
* Displays curated beginner vocabulary sequence
* Shows cross-language equivalents
* Supports editing
* Highlighting uses Master List membership
* Red = not in list or too early

Cognate Click Behavior
* Highlights matching tokens
* Inserts cognate into Master List

Project List Cognate Priority
* Cognates appear at top of Column C with green badge

Auto-Scroll for Master List
* Deferred until multilingual Master List is stable

Spanish as First Multilingual Target
* Clean frequency data
* Reliable lemma resources
* Strong cognate overlap
* Validates multilingual architecture

Frequency List Format
* Uniform structure:
  * rank
  * lemma

Lemma Map Format
* Flat inflected → lemma mapping
* No POS tags
* No metadata
* No nested objects

Highlight Logic Extension
* Language-aware
* Priority preserved:
  * Cognate → green
  * Known → black
  * Unknown → red

Replace Popup with Violations Panel
* Popups interrupt writing
* Panel provides stable diagnostics
* Supports sorting, grouping, toggling

4. Multilingual Decisions
Supported Languages
* English
* Spanish
* Koine Greek
* Latin

Dynamic Language Modules
* Load frequency, lemma, cognate files based on selected language
* Reduces memory usage
* Faster startup
* Cleaner architecture

5. Deployment Decisions
Render Hosting
* Free tier
* GitHub auto-deploy
* Easy environment variables

Supabase Hosting
* Persistent data
* Zero maintenance
* PostgreSQL reliability

6. Repository Structure Decisions
Use /docs Folder
* Organized repo
* Permanent memory
* Industry standard

Clean Rebuild in New Repo
* Avoid legacy clutter
* Remove Ruby artifacts
* Clean architecture

7. Future Considerations
Mobile Support (Deferred)
* Desktop UI optimized for writing
* Mobile requires:
  * Responsive layout
  * Collapsible panels
  * Touch interactions
  * Local caching
  * Optional PWA wrapper
* Revisit after Phase 5

Language Visibility Toggles (Deferred)
* Hide/show language columns in Master List
* Prevent UI clutter
* Supports multilingual workflows

Additional Future Options
* Cloud sync
* Kindle-ready export
* Image integration
* Animation script export
* Collaboration mode
* Advanced stats (reading level, repetition heatmap)

8. Debugging Decisions (Historical)
Frontend Fixes (2026-07-14)
* Fixed missing brace in renderViolationsPanel
* Repaired template string in renderFrequencyStats
* Removed nested duplicate renderFrequencyStats
* Verified global renderer functions

Backend Save/Load Fix (2026-07-15)
* Align frontend object shape with Supabase schema
* Map word → lemma on save
* Map lemma → word on load
* Added safety guard to normalizeLemma
* Confirmed correct API routes:
  * /api/master/save/:id
  * /api/master/load/:id

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
