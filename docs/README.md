# WordList Writer — README

## Overview
WordList Writer is a teacher‑facing authoring tool for creating controlled texts for beginner learners across multiple languages. It provides instructors with real‑time vocabulary analysis, curriculum alignment feedback, cognate scaffolding, and frequency‑based highlighting. Students never interact with the app directly; instead, teachers use WordList Writer to craft texts that are pedagogically sequenced, linguistically appropriate, and aligned with a chosen curriculum or master vocabulary list.

The tool supports two major instructional goals:
* Building a unified beginner‑level master vocabulary list across English, Spanish, Koine Greek, and Latin.
* Supporting multiple master vocabulary lists when a unified list is not feasible, while maintaining consistent analysis across all lists.

WordList Writer is designed to be flexible, extensible, and grounded in linguistic accuracy. Its normalization pipeline ensures consistent lemma handling across languages, while its cognate and frequency systems provide teachers with actionable insights during text creation. The final output delivered to students is a clean, controlled text without analytical overlays.

---

## Vision Statement
WordList Writer aims to become a professional authoring environment for teachers who create controlled texts for beginner learners across multiple languages. The tool provides curriculum‑aligned vocabulary control, frequency awareness, cognate scaffolding, and cross‑language lemma normalization. Its long‑term vision is to support teachers in building coherent beginner‑level curricula—either unified across languages or tailored to specific learner profiles—while maintaining a clean, teacher‑focused workflow that produces pedagogically appropriate texts for students.

---

## Quickstart Guide (Beginner‑Friendly)

### 1. Install Node.js
Download and install Node.js (LTS version) from:
https://nodejs.org

Verify installation:
node -v
npm -v

Code

### 2. Install Project Dependencies
Inside the project folder:
npm install

Code

This installs Express, Supabase client libraries, and other required packages.

### 3. Start the Local Server
Run:
node src/server.js

Code

Then open your browser to:
http://localhost:3000

Code

You should see the WordList Writer interface.

---

## How to Use the App (Beginner‑Friendly)

### Writing Window (Bottom Panel)
This is where you type your text.

As you type:
* Words are tokenized and normalized.
* Lemmas are detected.
* Cognates are highlighted **green**.
* Known words (frequency or project list) are **black**.
* Unknown words are marked with a **red asterisk**.

### Column A — Frequency List
Shows the NGSL (English) or other frequency list for the selected language.

Use it to:
* See which words are high‑frequency.
* Click a word to highlight it in your text.

### Column B — Cognates
Shows cognates for the selected language.

Clicking a cognate:
* Highlights all matching tokens in the writing window.
* Inserts the cognate into the Master List.

### Column C — Project Word List
Tracks all lemmas used in your current text.

Shows:
* Cognate tier
* Frequency tier
* Normalized lemma

### Column D — Master List
Your curriculum sequence.

You can:
* Add new lemmas
* Insert lemmas at specific ranks
* Reorder lemmas
* Add cross‑language equivalents
* Update cognate flags
* Update frequency metadata

Highlighting in the writing window uses this list.

### Violations Panel
Shows:
* Unknown words
* Curriculum gaps
* Out‑of‑order vocabulary

Updates in real time as you type.

### Saving Your Work
Click “Save Project.”

Behind the scenes:
1. Project metadata is saved.
2. Project wordlist is saved.
3. Master list is saved.
4. All IDs are propagated correctly.

### Loading Your Work
Click “Load Project.”

The app restores:
* Text
* Project wordlist
* Master list
* UI state

---

## Supabase Integration (Phase 2)
Supabase stores:
* Projects
* Project wordlists
* Master lists
* Cross‑language vocabulary

You must create a Supabase project and add your keys to environment variables.

Tables:
* projects  
* project_wordlists  
* master_wordlists  
* cross_language_master  

See `/docs/Supabase_Schema.md` for full details.

---

## Project Structure
docs/
public/
cognates/
frequency/
lemmas/
master/
backups/
src/
controllers/
lib/
routes/
utils/
scripts/

Code

---

## Documentation (Full System)
All documentation lives in `/docs`:

* [PRD](docs/PRD.md)  
* [Roadmap](docs/Roadmap.md)  
* [Tech Stack](docs/Tech_Stack.md)  
* [Data Sources](docs/Data_Sources.md)  
* [Decisions](docs/Decisions.md)  
* [Detailed Feature Specifications](docs/Detailed_Feature_Specifications.md)  
* [Supabase Schema](docs/Supabase_Schema.md)  
* [API Specification](docs/API_Specification.md)  
* [Glossary](docs/Glossary.md)  
* [Next Steps](docs/Next_Steps.md)  
* [Docs Index](docs/Docs_Index.md)  

These documents are restart‑proof and reconstruct the entire project.

---

## Development Status
Phase 1 — English‑only prototype: **Complete**  
Phase 2 — Supabase integration: **In progress**  
Phase 3 — Curriculum modeling: **Upcoming**  
Phase 4 — Multiple curriculum support: **Upcoming**  
Phase 5 — Teacher workflow enhancements: **Upcoming**  

---

## Notes for Future You
If you return after a long break:
1. Read this README first.  
2. Open `/docs/Docs_Index.md`.  
3. Review `/docs/Next_Steps.md`.  
4. Start coding again with full context restored.

---

## License
This project is for personal and educational use.

a Setup_Supabase.md (step‑by‑step guide to creating tables)

Just tell me what you want next.
