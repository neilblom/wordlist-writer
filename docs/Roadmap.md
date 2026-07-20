Roadmap — WordList Writer
Version: 2026-07-20
Status: Authoritative Source of Truth

Overview
This roadmap defines the complete development trajectory for WordList Writer. It merges the original plan with updated July 19 architecture rules, tokenizer rules, highlight pipeline rules, rendering rules, dictionary profile system, hybrid cognate window, alphabetical dictionary, cognate merging rules, Supabase v2 behaviors, and project ID fixes. It is restart-proof and reconstructs the entire multi-phase development sequence.

Phase 1 — Core Rebuild (English Only)
Step 1: normalize lemma pipeline
Step 2: normalize frequency pipeline
Step 3: normalize cognate pipeline
Step 4: implement centralized highlight pipeline
Step 5: implement project word list
Step 6: implement master list loading
Step 7: implement curriculum-order detection
Step 8: implement teacher-facing UI
Output:
* stable English-only authoring tool
* single-layer editor
* IME-safe input handling
* tier-aware highlighting

2026-07-15 Restart Plan (Phase 2 Entry Point)
Goal:
Fix backend save pipeline so projects, master lists, and wordlists save and load correctly.

Restart Checklist:
Step 1: confirm currentProjectId is set on new project and load project
Step 2: inspect /api/projects/save:
* does it require an ID?
* does it generate an ID?
* does it return an ID?
Step 3: inspect /api/projects/wordlist/save:
* does it require projectId?
* does it validate projectId?
Step 4: inspect saveEverything():
* does project save run first?
* does it capture returned ID?
* does it pass ID to wordlist save?
Step 5: test full save pipeline:
* create project
* save project
* reload page
* load project
* verify content, master list, wordlist, and dictionary profile restore correctly

Phase 2 — Multi-Language Foundations (Spanish, Latin, Greek)
Step 1: add Spanish lemma, frequency, cognate support
Step 2: add Latin lemma, cognate support
Step 3: add Koine Greek lemma, cognate support
Step 4: normalize cross-language pipelines
Step 5: add dictionary profile system
Step 6: add hybrid cognate window (detected + alphabetical)
Step 7: add pending → official cognate workflow
Step 8: add merged dictionary rebuild logic
Output:
* multi-language text analysis
* profile-aware cognate filtering
* alphabetical dictionary
* merged dictionary powering highlight pipeline

Phase 2 — Master List Stability (Completed)
Completed:
* save pipeline fixed (correct URL + mapping)
* load pipeline fixed (correct mapping back to UI)
* Supabase tables store correct rows
* dictionary profiles stored per project
* merged dictionary rebuilt on load
* UI renders loaded master list
Upcoming Milestones:
* implement global master list
* improve comparison logic
* add cognate tagging to master list
* begin multilingual expansion

Phase 3 — Curriculum Modeling
Step 1: build unified master vocabulary list prototype
Step 2: add curriculum-order warnings
Step 3: add curriculum violations panel
Step 4: add teacher controls for promoting/demoting vocabulary
Step 5: integrate dictionary profiles into curriculum modeling
Output:
* cross-linguistic beginner curriculum modeling
* unified or profile-specific curriculum options

Phase 4 — Multiple Curriculum Support
Step 1: add support for multiple master lists (age, subject, language)
Step 2: add master list switching
Step 3: add master list comparison tools
Step 4: add project-level vocabulary tracking across lists
Step 5: integrate merged dictionary into multi-curriculum workflows
Output:
* flexible system for teachers with different learner profiles

Phase 5 — Teacher Workflow Enhancements
Step 1: add text difficulty summary
Step 2: add readability metrics
Step 3: add NGSL/NAWL coverage metrics
Step 4: add Bible English List BEL (NGSL project)
Step 5: add cognate coverage metrics
Step 6: add export options (clean text, teacher report)
Step 7: add dictionary profile analytics
Output:
* professional authoring workflow for ESL and classical languages

Phase 6 — Optional Enhancements
Step 1: Supabase sync for master lists
Step 2: Supabase sync for cognate lists
Step 3: multi-teacher collaboration
Step 4: versioning for curriculum lists
Step 5: shared dictionary profiles across projects
Output:
* cloud-backed curriculum development studio

Phase 7 — Advanced Linguistic Features (Optional)
Step 1: multi-word cognate support
Step 2: phrase-level frequency analysis
Step 3: cross-language semantic clustering
Step 4: AI-assisted text simplification
Output:
* advanced linguistic tooling for expert instructors

Documentation Formatting Reminder
All documentation updates must follow this formatting standard:
* use plain text section titles
* use asterisks (*) for bullet points
* do not insert blank lines inside bullet lists
* use ASCII-only characters
* avoid Markdown headings (#)
* avoid fenced code blocks unless necessary
* use Step format for workflows to prevent GitHub auto-renumbering
This ensures consistent rendering across GitHub and prevents formatting breakage.
