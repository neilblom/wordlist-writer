WordList Writer — README
Version: 2026-07-20
Status: Authoritative Overview

Overview
WordList Writer is a teacher-facing authoring tool for creating controlled beginner-level texts across English, Spanish, Koine Greek, and Latin. It provides real-time vocabulary analysis, curriculum alignment feedback, cognate scaffolding, and frequency-based highlighting. Students never interact with the app. Teachers use WordList Writer to craft texts that are pedagogically sequenced, linguistically appropriate, and aligned with a chosen curriculum or master vocabulary list.

The tool supports two instructional goals:
* building a unified beginner-level master vocabulary list across languages
* supporting multiple master lists when a unified list is not feasible

WordList Writer uses a single contenteditable editor, centralized highlight pipeline, IME-safe input handling, and Supabase-backed persistence. The final output delivered to students is a clean text without analytical overlays.

Vision Statement
WordList Writer aims to become a professional authoring environment for teachers who create controlled texts for beginner learners across multiple languages. The tool provides curriculum-aligned vocabulary control, frequency awareness, cognate scaffolding, and cross-language lemma normalization. Its long-term vision is to support teachers in building coherent beginner-level curricula while maintaining a clean, teacher-focused workflow.

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

Column B — Cognates
Shows cognates for the selected language.
Clicking a cognate:
* highlights matching tokens
* inserts cognate into the Master List

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
Step 2: project id propagated
Step 3: project wordlist saved
Step 4: master list saved
Rules:
* masterList must contain plain strings
* project-id-input must always be updated

Loading Your Work
Click Load Project.
The app restores:
* text
* project wordlist
* master list
* UI state
Highlight pipeline runs automatically.

Supabase Integration
Supabase stores:
* projects
* project_wordlists
* master_wordlists
* cross_language_master
Tables:
* projects
* project_wordlists
* master_wordlists
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
Phase 2: Supabase integration — in progress
Phase 3: curriculum modeling — upcoming
Phase 4: multiple curriculum support — upcoming
Phase 5: teacher workflow enhancements — upcoming

Notes for Future You
Step 1: read this README
Step 2: open docs/Docs_Index.md
Step 3: review docs/Next_Steps.md
Step 4: resume development with full context

License
This project is for personal and educational use.
