Roadmap — WordList Writer
Version: 2026-07-17
Status: Authoritative Source of Truth

Phase 1 — Core Rebuild (English Only)
Step 1: Normalize lemma pipeline
Step 2: Normalize frequency pipeline
Step 3: Normalize cognate pipeline
Step 4: Implement highlight loop
Step 5: Implement project list
Step 6: Implement master list loading
Step 7: Implement curriculum-order detection
Step 8: Implement teacher-facing UI
Output: Stable English-only authoring tool

2026-07-15 Restart Plan (Phase 2 Entry Point)
Goal:
Fix backend save pipeline so projects and wordlists save and load correctly.

Restart Checklist:
Step 1: Confirm currentProjectId is set on new project and load project.
Step 2: Inspect /api/projects/save:
* Does it require an ID?
* Does it generate an ID?
* Does it return an ID?
Step 3: Inspect /api/projects/wordlist/save:
* Does it require projectId?
* Does it validate projectId?
Step 4: Inspect saveEverything():
* Does project save run first?
* Does it capture returned ID?
* Does it pass ID to wordlist save?
Step 5: Test full save pipeline:
* Create project
* Save project
* Reload page
* Load project
* Verify content, master list, and wordlist restore correctly

Phase 2 — Multi-Language Foundations (Spanish, Latin, Greek)
Step 1: Add Spanish lemma, frequency, cognate support
Step 2: Add Latin lemma, cognate support
Step 3: Add Koine Greek lemma, cognate support
Step 4: Normalize cross-language pipelines
Output: Multi-language text analysis for teacher use

Phase 2 — Master List Stability (Completed)
Completed:
* Save pipeline fixed (correct URL + mapping)
* Load pipeline fixed (correct mapping back to UI)
* Supabase tables store correct rows
* UI renders loaded Master List

Upcoming Milestones:
* Implement Global Master List
* Improve comparison logic
* Add cognate tagging to Master List
* Begin multilingual expansion

Phase 3 — Curriculum Modeling
Step 1: Build unified master vocabulary list prototype
Step 2: Add curriculum-order warnings
Step 3: Add curriculum violations panel
Step 4: Add teacher controls for promoting/demoting vocabulary
Output: Cross-linguistic beginner curriculum modeling

Phase 4 — Multiple Curriculum Support
Step 1: Add support for multiple master lists (age, subject, language)
Step 2: Add master list switching
Step 3: Add master list comparison tools
Step 4: Add project-level vocabulary tracking across lists
Output: Flexible system for teachers with different learner profiles

Phase 5 — Teacher Workflow Enhancements
Step 1: Add text difficulty summary
Step 2: Add readability metrics
Step 3: Add NGSL/NAWL coverage metrics
Step 4: Add Bible English List BEL, part of the NGSL project
Step 5: Add cognate coverage metrics
Step 6: Add export options (clean text, teacher report)
Output: Professional authoring workflow for ESL and classical languages

Phase 6 — Optional Enhancements
Step 1: Supabase sync for master lists
Step 2: Supabase sync for cognate lists
Step 3: Multi-teacher collaboration
Step 4: Versioning for curriculum lists
Output: Cloud-backed curriculum development studio

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
