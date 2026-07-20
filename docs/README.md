WordList Writer — README
Version: 2026-07-20
Status: Authoritative Overview

Overview
WordList Writer is a teacher-facing authoring tool for creating controlled beginner-level texts across English, Spanish, Koine Greek, and Latin. It provides real-time vocabulary analysis, curriculum alignment feedback, cognate scaffolding, dictionary profiles, and frequency-based highlighting. Students never interact with the app. Teachers use WordList Writer to craft texts that are pedagogically sequenced, linguistically appropriate, and aligned with a chosen curriculum or master vocabulary list.

The tool supports two instructional goals:
* building a unified beginner-level master vocabulary list across languages
* supporting multiple master lists when a unified list is not feasible

WordList Writer uses a single contenteditable editor, centralized highlight pipeline, IME-safe input handling, hybrid cognate window, alphabetical dictionary, and Supabase-backed persistence. The final output delivered to students is a clean text without analytical overlays.

Vision Statement
WordList Writer aims to become a professional authoring environment for teachers who create controlled texts for beginner learners across multiple languages. The tool provides curriculum-aligned vocabulary control, frequency awareness, cognate scaffolding, dictionary profiles, and cross-language lemma normalization. Its long-term vision is to support teachers in building coherent beginner-level curricula while maintaining a clean, teacher-focused workflow.

Quickstart Guide

Step 1: Install Node.js
* install Node.js LTS from https://nodejs.org
* verify installation using:
  node -v
  npm -v

Step 2: Install Project Dependencies
* inside the project folder:
  npm install

Step 3: Start the Local Server
* run:
  node src/server.js
* open browser:
  http://localhost:3000

You should see the WordList Writer interface.

How to Use the App

Writing Window
The writing window is a single contenteditable element. As you type:
* words are tokenized and normalized
* cognates highlight green
* known words display normally
* unknown words show a red asterisk
* violations update in real time
Rules:
* highlight updates occur only through requestHighlightUpdate
* no highlight during IME composition

Column A — Frequency List
Shows NGSL or other frequency lists.
Use it to:
* check word frequency
* click a word to highlight it in the text

Column B — Cognates (Hybrid Window)
Shows cognates filtered by dictionary profile.
Contains:
* detected cognates (top)
* alphabetical dictionary (bottom)
Clicking a cognate:
* highlights matching tokens
* inserts cognate into the Master List
You may also:
* add new cognates
* edit pending cognates
* publish pending cognates into official dictionary

Dictionary Profile Dropdown
Profiles:
* spanish
* latin
* greek
* merged
Profile determines which cognates appear in Column B.

Column C — Project Word List
Tracks all lemmas used in the current text.
Shows:
* cognate tier
* frequency tier
* normalized lemma

Column D — Master List
Your curriculum sequence.
You can:
* add lemmas
* insert lemmas at specific ranks
* reorder lemmas
* add cross-language equivalents
Highlighting depends on this list.

Violations Panel
Shows:
* unknown words
* curriculum gaps
* out-of-order vocabulary
Updates automatically as you type.

Saving Your Work
Click Save Project.
Behind the scenes:
Step 1: project metadata saved  
Step 2: dictionary profile saved  
Step 3: project id propagated  
Step 4: project wordlist saved  
Step 5: master list saved  
Rules:
* masterList must contain plain strings
* project-id-input must always be updated

Loading Your Work
Click Load Project.
The app restores:
* text
* dictionary profile
* project wordlist
* master list
* cognates (official and pending)
* merged dictionary
* UI state
Highlight pipeline runs automatically.

Supabase Integration
Supabase stores:
* projects
* project_wordlists
* master_wordlists
* cognates_official
* cognates_pending
* dictionary_profiles
* cross_language_master
Supabase v2 requires .select() to retrieve inserted rows.
See docs/Supabase_Schema.md for details.

Project Structure
docs/
public/
frequency/
lemmas/
cognates/
master/
backups/
src/
controllers/
lib/
routes/
utils/
scripts/

Documentation
All documentation lives in /docs:
* PRD
* Roadmap
* Tech Stack
* Frontend Architecture
* Backend Architecture
* Rendering Pipeline
* Tokenizer Specification
* Master List Workflow
* Developer Workflow
* Teacher Workflow
* Data Sources
* Decisions
* Detailed Feature Specifications
* Supabase Schema
* API Specification
* Glossary
* Next Steps
* Docs Index

Development Status
Phase 1: English-only prototype — complete  
Phase 2: Supabase integration — complete  
Phase 3: curriculum modeling — in progress  
Phase 4: multiple curriculum support — upcoming  
Phase 5: teacher workflow enhancements — upcoming

Notes for Future You
Step 1: read this README  
Step 2: open docs/Docs_Index.md  
Step 3: review docs/Next_Steps.md  
Step 4: resume development with full context

License
This project is for personal and educational use.
