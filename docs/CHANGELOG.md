CHANGELOG — WordList Writer
Version: 2026-07-17
Status: Authoritative Project History

Overview
This changelog records all meaningful changes to WordList Writer, including feature additions, bug fixes, architectural decisions, documentation updates, and backend schema changes. It is designed to help future developers and future sessions reconstruct the evolution of the project.

2026-07-10 — Initial Project Setup
* Created new clean repository
* Added base folder structure (docs, public, src, scripts)
* Added initial PRD draft
* Added Tech Stack outline
* Added early tokenizer prototype

2026-07-11 — Highlight Pipeline Foundation
* Implemented tokenizer with punctuation preservation
* Added lemma normalization pipeline
* Added frequency list integration (NGSL)
* Added basic highlight rules (green, black, red)
* Added project list prototype

2026-07-12 — Cognate System (Phase 1)
* Added cognate JSON files
* Implemented unified COGNATE_MAP
* Added tier metadata (latin, greek, biblical, general)
* Added tier-aware highlighting
* Added tier-aware tooltips

2026-07-13 — Three-Column UI
* Implemented Column A (Frequency)
* Implemented Column B (Cognates)
* Implemented Column C (Project List)
* Added basic CSS layout
* Added writing window overlay system

2026-07-14 — Violations Panel + Major Debugging
* Replaced popup warnings with persistent violations panel
* Fixed missing brace in renderViolationsPanel
* Fixed broken template string in renderFrequencyStats
* Removed nested duplicate renderer
* Restored highlight pipeline functionality
* Restored project list functionality
* Restored violations panel functionality

2026-07-15 — Master List Save/Load Pipeline Fix
* Aligned frontend shape with Supabase schema
* Added mapping: word → lemma on save
* Added mapping: lemma → word on load
* Added safety guard to normalizeLemma
* Confirmed correct API routes (/api/master/save/:id, /api/master/load/:id)
* Fixed ID propagation issues in saveEverything()
* Fixed null projectId errors
* Fixed ordering of save operations

2026-07-16 — Documentation Overhaul (Phase 1 Complete)
* Rewrote PRD in professional format
* Added Roadmap with dependency chain
* Added Tech_Stack.md
* Added Data_Sources.md
* Added Decisions.md (architectural history)
* Added Detailed_Feature_Specifications.md
* Added Glossary.md
* Added Next_Steps.md
* Added Docs_Index.md

2026-07-17 — Backend Architecture Formalization
* Added Supabase_Schema.md (full database blueprint)
* Added API_Specification.md (backend contract)
* Added Setup_Supabase.md (beginner-friendly setup guide)
* Added README.md with project introduction and usage guide
* Added CHANGELOG.md (this file)

Planned (Phase 2 — Supabase Integration)
* Implement full save/load pipeline
* Add project switching
* Add master list editing UI
* Add frequency ingestion validation
* Add multilingual lemma mapping
* Add error handling for malformed rows

Planned (Phase 3 — Multilingual Expansion)
* Add Spanish frequency + lemma + cognates
* Add Latin frequency + lemma + cognates
* Add Greek frequency + lemma + cognates
* Add language switching UI
* Add cross-language master list editing

Planned (Phase 4 — Curriculum Modeling)
* Add curriculum-order detection
* Add severity levels for violations
* Add curriculum diagnostics summary

Planned (Phase 5 — Teacher Tools)
* Add export clean text
* Add vocabulary export
* Add quiz generator
* Add reading-level metrics

Summary
This changelog provides a complete historical record of WordList Writer’s development. It is essential for understanding architectural decisions, debugging past issues, and reconstructing project context after long breaks.

Documentation Formatting Reminder
* Use plain text section titles
* Use asterisks (*) for bullet points
* Do not insert blank lines inside bullet lists
* Use ASCII-only characters
* Avoid Markdown headings (#)
* Avoid fenced code blocks unless necessary
* Use Step format for workflows
