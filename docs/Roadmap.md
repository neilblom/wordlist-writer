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
