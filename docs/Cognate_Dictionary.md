Cognate Dictionary
Version: 2026-07-20
Status: Authoritative Cognate System Guide

Overview
This document describes the cognate dictionary architecture for WordList Writer. It explains how cognates are stored, how dictionary profiles work, how the hybrid cognate window behaves, how pending and official cognates are merged, and how the system interacts with the highlight pipeline and master list. The cognate system is teacher-facing only and is designed to support multilingual curriculum design.

1. Cognate System Identity
Core principles:
* cognate tools are for teachers only
* students never see cognate UI
* cognates support curriculum design and text authoring
* system must avoid manual JSON editing
* system must integrate cleanly with highlight pipeline and master list

2. Data Structures

2.1 Official Cognates
Official cognates:
* stored in cognates_official table or JSON
* represent published, stable entries
* used for highlight and dictionary display
Fields:
* projectId or global scope
* profile (spanish, latin, greek, merged)
* baseLanguage (text language)
* lemma (normalized base word)
* cognate (normalized cognate form)
* tier (optional difficulty or curriculum tier)

2.2 Pending Cognates
Pending cognates:
* stored in cognates_pending table or JSON
* represent newly added or edited entries
* not yet published into official dictionary
Fields:
* projectId
* profile
* baseLanguage
* lemma
* cognate
* tier

2.3 Merged Dictionary
Merged dictionary:
* created at startup and after publish
* combines official and pending cognates
* used by highlight pipeline and hybrid window
Rules:
* pending entries override official entries for same lemma and profile
* merged dictionary is read-only at runtime
* frontend does not write directly to merged dictionary

3. Dictionary Profiles

3.1 Profile Types
Supported dictionary profiles:
* spanish
* latin
* greek
* merged
Profile meaning:
* spanish: cognates for Spanish-speaking learners
* latin: academic and classical roots
* greek: STEM and scientific roots
* merged: combined view for brainstorming and curriculum design

3.2 Per-Project Profiles
Profile rules:
* dictionaryProfile is stored per project
* each project remembers its last used profile
* profile affects cognate filtering and display
* profile does not change project language
Project language:
* controlled by separate Language dropdown
* represents text output language (English, Spanish, Latin, Greek)

4. Hybrid Cognate Window

4.1 Window Structure
Hybrid window:
* top section: detected cognates in current text
* bottom section: full alphabetical dictionary
Both sections:
* filtered by active dictionaryProfile
* use merged dictionary as source

4.2 Top Section: Detected Cognates
Detected cognates:
* show only lemmas present in current text
* sorted alphabetically
* each row shows:
* lemma
* cognate
* tier
* profile
Interactions:
* click to highlight tokens in writing window
* click to insert cognate into master list
* click to edit cognate (pending)

4.3 Bottom Section: Full Alphabetical Dictionary
Full dictionary:
* shows all cognates for active profile
* sorted alphabetically by lemma
* includes both official and pending entries
Each row shows:
* lemma
* cognate
* tier
* source (official or pending)
Interactions:
* add new cognate
* edit existing cognate
* delete pending cognate
* publish pending cognates via button

5. Cognate Workflows

5.1 Add Cognate Workflow
Step 1: teacher clicks Add Cognate in hybrid window
Step 2: UI prompts for lemma, cognate, tier, profile
Step 3: frontend sends data to /api/cognates/add
Step 4: backend writes entry to cognates_pending
Step 5: frontend reloads cognate data
Step 6: frontend calls requestHighlightUpdate to refresh highlights

5.2 Edit Cognate Workflow
Step 1: teacher clicks Edit on a pending cognate
Step 2: UI shows current lemma, cognate, tier
Step 3: teacher edits fields
Step 4: frontend sends data to /api/cognates/edit
Step 5: backend updates cognates_pending
Step 6: frontend reloads cognate data
Step 7: frontend calls requestHighlightUpdate

5.3 Publish Cognates Workflow
Step 1: teacher clicks Publish Cognates for active profile
Step 2: frontend calls /api/cognates/publish with projectId and profile
Step 3: backend loads official and pending cognates
Step 4: backend merges pending into official
Step 5: backend clears pending entries for that project and profile
Step 6: backend returns success
Step 7: frontend reloads cognate data and merged dictionary
Step 8: frontend calls requestHighlightUpdate to refresh highlights and hybrid window

6. Highlight Integration

6.1 Highlight Pipeline Rules
Highlight rules:
* highlight pipeline uses merged dictionary
* requestHighlightUpdate is the only entry point
* updateCognates must call requestHighlightUpdate
* handleStableInput must not be called directly by cognate functions
* no highlight runs during IME composition

6.2 Cognate Styling
Styling:
* cognate tokens receive green underline via CSS class
* unknown words receive red asterisk via CSS pseudo-element
* known frequency words receive normal styling
* master list words receive curriculum styling
Cognate detection:
* based on normalized lemmas
* filtered by active dictionaryProfile

7. Master List Integration
Master list interactions:
* clicking a cognate may insert lemma into master list
* master list updates may trigger requestHighlightUpdate
* master list remains plain strings
Cross-language integration:
* master list may reference cross-language equivalents
* cognate flags indicate presence of cognates in other languages
Rules:
* master list is not a known-word list
* known words come from frequencySet
* cognates are separate from frequency and master list logic

8. Startup Behavior

8.1 Cognate Loading
Startup steps for cognates:
* load cognate JSON or Supabase data for official entries
* load pending cognates for active project
* load dictionaryProfile for active project
* build merged dictionary
* render hybrid cognate window
* attach event handlers for add, edit, publish

8.2 Profile-Aware Rendering
Profile rules at startup:
* dictionaryProfile determines which cognates are visible
* detected section uses merged dictionary filtered by profile and text tokens
* full dictionary section uses merged dictionary filtered by profile only

9. Invariants and Safety Rules
Cognate invariants:
* pending entries must never be used directly for highlight without merging
* merged dictionary must be rebuilt after publish
* dictionaryProfile must be respected in all cognate queries
* no direct DOM mutation outside render functions
* no direct calls to handleStableInput from cognate code
Safety rules:
* avoid manual JSON editing for cognates
* avoid mixing backend and frontend logic
* keep cognate UI teacher-only
* keep profile and language dropdowns conceptually separate

10. Summary
This cognate dictionary document defines the architecture of the cognate subsystem in WordList Writer. It explains data structures, dictionary profiles, hybrid window behavior, pending and official merging, highlight integration, and master list interactions. It is essential for maintaining a stable, teacher-facing cognate system as the application evolves.
