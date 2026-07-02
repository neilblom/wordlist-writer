# 📄 **Roadmap.md — WordList Writer (Node.js + Express + Supabase)**

## Overview  
This roadmap outlines the development phases for the WordList Writer rebuild. Each phase is designed to be small, achievable, and token‑friendly, ensuring steady progress without overwhelming complexity.

---

## **Phase 1 — Core Rebuild (English Only)**  
**Goal:** Recreate the essential functionality using Node.js + Express with English as the only active language.

- Set up Node.js + Express project structure  
- Serve static frontend (HTML/CSS/JS)  
- Load English frequency list (NGSL)  
- Load English lemma map  
- Implement tokenizer  
- Implement lemma lookup  
- Implement highlighting logic (red/black only)  
- Build writing window  
- Build frequency list window  

**Deliverable:** A working English‑only prototype with correct highlighting.

---

## **Phase 2 — Cognate Window**  
**Goal:** Add cognate awareness and the green highlighting layer.

- Add English ↔ Spanish cognate list  
- Add English ↔ Latin cognate list  
- Add English ↔ Greek cognate list  
- Build cognate window (Column B)  
- Add green highlighting (highest priority)  
- Add click‑to‑highlight behavior  

**Deliverable:** Cognate window fully functional with correct priority rules.

Phase 2.4 — Cognate Interaction Enhancements
Status: Complete
Description: Adds interactive behavior to Column B (Cognate Window). Clicking a cognate now highlights all matching tokens in the writing window and inserts the cognate into the Master List (Column D). This creates a direct link between discovery (Column B) and curation (Column D).
✔ Phase 2 Completed
Cognate Window implemented

Automatic ✓ cognate marker

Click‑to‑add cognate → Master List

Correct insertion logic (after last Master List lemma in story)

Warning system implemented

Master List re-renders with correct ranks

Project List remains usage‑based only

🔜 Phase 3 (Next)
Manual reordering UI improvements

Export/import Master List

“Unused cognates” view

“Next expected word” sidebar

Multi‑story project support

Save per‑story Project Lists

Deliverables:

Click‑to‑highlight cognate in writing window

Click‑to‑insert cognate into Master List

Automatic rank assignment

Duplicate prevention

Tooltip integration with cognateMap

Project List (Column C) cognate priority sorting

Phase 2.5 — UI Polish (Deferred)
Status: Deferred to post‑Phase 3
Description: Minor UI enhancements planned for later, after multilingual support is added.

Planned Enhancements:

Auto‑scroll Column D to newly added Master List items

Visual animation when a cognate is added

Optional “Add All Cognates” batch button in Column B

Reason for Deferral:  
These enhancements depend on stable multilingual behavior introduced in Phase 3. Deferring avoids rework and ensures UI polish is applied once the full cross‑language pipeline is active.

---

## **Phase 3 — Multilingual Support**  
**Goal:** Expand the system to support Spanish, Koine Greek, and Latin.

- Add frequency lists for Spanish, Greek, Latin  
- Add lemma maps for each language  
- Add cognate lists for each language pair  
- Add language selector UI  
- Load correct modules based on selected language  
- Update tokenizer for Greek/Latin Unicode rules  

**Deliverable:** All four languages fully supported.
Phase 3: Master List UI
Add Column D to the top panel.

Display Master List with rank and multilingual equivalents.

Add “Add to Master List” and “Insert at Rank” controls.

Add “Move Up/Down” controls.

Add cognate flag editing.

Add frequency metadata display.

Integrate Master List with highlighting logic.

---

Phase 3.1 — Spanish Frequency + Lemma Map
Status: In Progress
Description: Introduces full Spanish support into the lexical pipeline. This includes loading a Spanish frequency list, loading a Spanish lemma map, and extending highlight logic to support Spanish tokens. Spanish becomes the first non‑English language with full frequency + lemma + cognate integration.

Deliverables:

/frequency/spanish.json (ranked list of Spanish lemmas)

/lemmas/spanish.json (inflected → lemma mapping)

Spanish-aware tokenizer adjustments (if needed)

Spanish highlight logic (black = known, red = unknown, green = cognate)

Spanish frequency integration into Master List

Spanish lemma integration into Project List

Spanish cognate integration into Column B

Notes:  
Spanish is the template language for Phase 3.2 (Latin + Greek). All architectural decisions here must support future languages without refactoring.

Phase 3.2 — Latin + Greek Frequency + Lemma Maps
Status: Planned
Description: Adds classical languages to the pipeline using the same architecture established in Phase 3.1. Latin and Greek frequency lists will be smaller and more curated, but follow the same JSON structure.

Phase 3.3 — Dynamic Language Switcher
Status: Planned
Description: Adds UI controls allowing the user to select the active language. Highlight logic, frequency lists, lemma maps, and cognate detection adapt instantly.

---

## **Phase 4 — Supabase Integration**  
**Goal:** Add persistent storage for projects and vocabulary tracking.

### Projects  
- Create `projects` table  
- Create `project_wordlists` table  
- Save/load project text  
- Save/load project word list  

### Master Lists  
- Create `master_wordlists` table  
- Track first‑seen words  
- Update master list automatically  

### Cross‑Language Master List  
- Create master_list table.
- Store cognate relationships  
- Store frequency ranks  
- Store lemma mappings
- Sync Master List edits to Supabase.
- Load Master List on startup.
- Ensure highlighting uses Supabase‑backed Master List.
- Add cross‑language frequency lists.
- Add multilingual cognate mapping.

**Deliverable:** User data persists across sessions and devices.

### Phase 4 — Master List Enhancements
- Add language visibility toggles
- Allow user to show/hide individual language columns
- Support future languages without changing the renderer
- Maintain fixed rank-based ordering regardless of visibility

---

## **Phase 5 — Deployment**  
**Goal:** Deploy the app publicly.

- Deploy Node.js backend to Render  
- Connect Render to Supabase  
- Configure environment variables  
- Test all endpoints  
- Test multilingual behavior  
- Test Supabase syncing  
- Final UI polish  

**Deliverable:** Publicly accessible, stable version of WordList Writer.

---

## **Phase 6 — Optional Enhancements**  
These are not required for the MVP but are planned future improvements.

- Export project word lists  
- Import custom lists  
- Add dark mode  
- Add mobile layout  
- Add spaced‑repetition review mode  
- Add “word difficulty heatmap”  
- Add user accounts (Supabase Auth)
