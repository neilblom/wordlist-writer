Phase 1 — Core Rebuild (English Only)
- Normalize lemma pipeline
- Normalize frequency pipeline
- Normalize cognate pipeline
- Implement highlight loop
- Implement project list
- Implement master list loading
- Implement curriculum‑order detection
- Implement teacher‑facing UI
- Output: A stable English‑only authoring tool

## 2026-07-15 — Restart Plan

### Goal
Fix backend save pipeline so projects and wordlists save and load correctly.

### Starting Prompt for Copilot
“Restarting Phase 2. We need to fix the backend save pipeline: projectId null, project save order, and wordlist save. Begin by checking how currentProjectId is set and how the save endpoints expect it.”

### Checklist
1. Confirm `currentProjectId` is set on new project + load project.
2. Inspect `/api/projects/save`:
   - Does it require an ID?
   - Does it generate an ID?
   - Does it return an ID?
3. Inspect `/api/projects/wordlist/save`:
   - Does it require `projectId`?
   - Does it validate it?
4. Inspect `saveEverything()`:
   - Does it call project save first?
   - Does it capture returned ID?
   - Does it pass ID to wordlist save?
5. Test full save pipeline:
   - Create project
   - Save project
   - Reload page
   - Load project
   - Verify content + master list + wordlist restore correctly.


Phase 2 — Multi‑Language Foundations (Spanish, Latin, Greek)
- Add Spanish lemma + frequency + cognate support
- Add Latin lemma + cognate support
- Add Koine Greek lemma + cognate support
- Normalize cross‑language pipelines
- Output: Multi‑language text analysis for teacher use

Phase 2 — Master List Stability Completed
   Completed
      Save pipeline fixed (correct URL + correct mapping).
      Load pipeline fixed (correct mapping back to UI).
      Supabase table now stores correct rows.
      UI now renders loaded Master List.
   Upcoming Milestones
      Implement Global Master List.
      Improve comparison logic.
      Add cognate tagging to Master List.
      Begin multilingual expansion.

Phase 3 — Curriculum Modeling
- Build unified master vocabulary list prototype
- Add curriculum‑order warnings
- Add “curriculum violations” panel
- Add teacher controls for promoting/demoting vocabulary
- Output: A tool that helps build a cross‑linguistic beginner curriculum

Phase 4 — Multiple Curriculum Support
- Add support for multiple master lists (age, subject, language)
- Add master list switching
- Add master list comparison tools
- Add project‑level vocabulary tracking across lists
- Output: A flexible system for teachers with different learner profiles

Phase 5 — Teacher Workflow Enhancements
- Add text difficulty summary
- Add readability metrics
- Add NGSL/NAWL coverage metrics
- Add cognate coverage metrics
- Add export options (clean text, teacher report)
- Output: A professional authoring workflow for ESL and classical languages

Phase 6 — Optional Enhancements
- Supabase sync for master lists
- Supabase sync for cognate lists
- Multi‑teacher collaboration
- Versioning for curriculum lists
- Output: Cloud‑backed curriculum development studio

