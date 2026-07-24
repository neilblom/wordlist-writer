Roadmap — WordList Writer
Version: 2026-07-23
Status: Authoritative Source of Truth

Overview
This roadmap defines the complete development trajectory for WordList Writer. It merges the original plan with updated July 19 architecture rules, tokenizer rules, highlight pipeline rules, rendering rules, hybrid master list model, Supabase v2 behaviors, and project ID fixes. All Phase 3 dictionary profile, alphabetical dictionary, merged dictionary, and cognate publishing features have been removed. This roadmap is restart-proof and reconstructs the entire multi-phase development sequence.

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
* basic cognate detection
* curriculum-order warnings

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
* verify content, master list, and wordlist restore correctly

Phase 2 — Multi-Language Foundations (Simplified)
Step 1: add Spanish lemma and frequency support
Step 2: add Spanish cognate detection (simple window)
Step 3: normalize cross-language pipelines
Step 4: stabilize English-only master list
Step 5: implement story-order insertion logic
Step 6: remove dictionary profiles until Phase 3
Step 7: remove alphabetical dictionary until Phase 3
Step 8: remove pending → official cognate workflow until Phase 3
Output:
* stable English-only master list
* simple cognate window
* Spanish cognate detection
* predictable normalization
* stable highlight pipeline

Phase 2 — Master List Stability (Completed)
Completed:
* save pipeline fixed (correct URL + mapping)
* load pipeline fixed (correct mapping back to UI)
* Supabase tables store correct rows
* master list loads correctly
* project list loads correctly
* highlight pipeline stable
Upcoming Milestones:
* implement global master list
* improve comparison logic
* begin multilingual expansion

Phase 3 — Curriculum Modeling
Step 1: build unified master vocabulary list prototype
Step 2: add curriculum-order warnings
Step 3: add curriculum violations panel
Step 4: add teacher controls for promoting/demoting vocabulary
Output:
* cross-linguistic beginner curriculum modeling
* unified curriculum options

Phase 4 — Multiple Curriculum Support
Step 1: add support for multiple master lists (age, subject, language)
Step 2: add master list switching
Step 3: add master list comparison tools
Step 4: add project-level vocabulary tracking across lists
Output:
* flexible system for teachers with different learner profiles

Phase 5 — Teacher Workflow Enhancements
Step 1: add text difficulty summary
Step 2: add readability metrics
Step 3: add NGSL/NAWL coverage metrics
Step 4: add Bible English List BEL (NGSL project)
Step 5: add cognate coverage metrics
Step 6: add export options (clean text, teacher report)
Output:
* professional authoring workflow for ESL and classical languages

Phase 6 — Optional Enhancements
Step 1: Supabase sync for master lists
Step 2: Supabase sync for cognate lists
Step 3: multi-teacher collaboration
Step 4: versioning for curriculum lists
Output:
* cloud-backed curriculum development studio

Phase 7 — Advanced Linguistic Features (Optional)
Step 1: multi-word cognate support
Step 2: phrase-level frequency analysis
Step 3: cross-language semantic clustering
Step 4: AI-assisted text simplification
Output:
* advanced linguistic tooling for expert instructors

Other features:
Assignment Mode
* Assignment Mode is used when the teacher wants students to learn and use a small, controlled set of vocabulary items.
* The master list contains a short set of lemmas, typically 10 to 30 words.
* The story is expected to use these lemmas in the intended sequence.
* The system checks for missing lemmas, extra lemmas, and out-of-order lemmas.
* The highlight pipeline marks known lemmas according to the assignment list.
* Unknown lemmas are flagged for teacher review.
* Order checking is strict in this mode and is used to ensure controlled text production.

Curriculum Mode
* Curriculum Mode is used when the teacher wants to evaluate a text against a larger vocabulary set, typically 50 to 200+ words.
* The master list contains the full curriculum vocabulary for a unit, semester, or course.
* The story is evaluated for coverage, unknown words, and general alignment with the curriculum.
* Order checking is optional or disabled because sequence is less important for large lists.
* The highlight pipeline marks all known lemmas from the curriculum list.
* Unknown lemmas indicate areas where the text exceeds the learner's studied vocabulary.
* This mode is used for level checking, text selection, and curriculum alignment.

Order Checking Summary
* Order checking ensures that the story uses vocabulary in the same sequence as the ranked master list.
* Each lemma in the master list has a rank based on its position in the list.
* The story is converted into a sequence of normalized lemmas.
* Each lemma in the story is mapped to its rank if it exists in the master list.
* If the rank sequence ever decreases, the story is out of order.
* Missing lemmas are flagged for teacher review.
* Lemmas that appear too early or too late can be flagged depending on mode.
* Order checking is most useful in Assignment Mode and optional in Curriculum Mode.
* Order checking is language-agnostic and can be added after multilingual support because it depends only on normalized lemma ranks.

Workflow for Assignment Mode
Step 1: Create a new project containing the assignment vocabulary list.
Step 2: Add the target lemmas to the master list in the intended teaching order.
Step 3: Paste or write the story in the editor.
Step 4: Review highlights, missing lemmas, and unknown lemmas.
Step 5: Review order checking results to ensure the story follows the intended sequence.
Step 6: Save the project for future reuse or modification.

Workflow for Curriculum Mode
Step 1: Create a new project containing the curriculum vocabulary list.
Step 2: Add all lemmas from the curriculum into the master list.
Step 3: Paste or load the story in the editor.
Step 4: Review highlights and unknown lemmas to determine text suitability.
Step 5: Review coverage statistics to evaluate alignment with the curriculum.
Step 6: Save the project for future level checking or text evaluation.


Documentation Formatting Reminder
All documentation updates must follow this formatting standard:
* use plain text section titles
* use asterks (*) for bullet points
* do not insert blank lines inside bullet lists
* use ASCII-only characters
* avoid Markdown headings (#)
* avoid fenced code blocks unless necessary
* use Step format for workflows to prevent GitHub auto-renumbering
This ensures consistent rendering across GitHub and prevents formatting breakage.
