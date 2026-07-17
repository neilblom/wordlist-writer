Product Requirements Document (PRD) - WordList Writer
Version: 2026-07-17
Status: Authoritative Source of Truth
Owner: Neil Blom
Scope: Teacher-Only Vocabulary Authoring Tool

1. Teacher-Only Scope Anchor (Critical)
WordList Writer is a teacher-only text authoring tool for creating beginner-level reading materials in English, Spanish, Latin, and Koine Greek.
It is not a student-facing app, not a learning platform, and not a general text editor.
The app exists for one purpose:
To help teachers write texts using a controlled beginner vocabulary list across multiple languages.
All features, UI decisions, pipelines, and future enhancements must reinforce this scope.

2. Product Overview
WordList Writer analyzes text in real time and highlights vocabulary based on curriculum criteria. It provides teachers with immediate feedback about:
* Allowed vs. disallowed vocabulary
* Cognates
* Frequency list membership
* Curriculum violations
* Project-specific vocabulary usage
* Master list alignment

3. Core User Workflow
Step 1: Teacher selects a language (English, Spanish, Latin, Greek).
Step 2: Teacher writes text in the writing window.
Step 3: The app highlights each word according to curriculum rules.
Step 4: The top panel displays four reference lists:
* Column A: Frequency List
* Column B: Cognates
* Column C: Project Word List
* Column D: Master List
Step 5: The violations panel shows curriculum mismatches.
Step 6: Teacher saves the project and its wordlist to Supabase.
Step 7: Teacher loads previous projects to continue writing.

4. Non-Goals
WordList Writer does not:
* Teach students
* Provide quizzes or exercises
* Generate translations
* Act as a dictionary
* Act as a grammar checker
* Provide AI text generation
* Support arbitrary languages
* Support advanced NLP
* Support student accounts or classroom management

5. Functional Requirements

5.1 Highlighting Rules (Strict Priority)
Step 1: Green - Cognate
Step 2: Black - Known (frequency list or project list)
Step 3: Red - Unknown (not in master list)

5.2 Tokenization Requirements
* English and Spanish use whitespace + punctuation
* Greek and Latin use Unicode-aware tokenization + accent stripping
* Tokens must preserve punctuation
* Non-word tokens pass through unchanged

5.3 Lemma Lookup Requirements
* All tokens normalize via normalizeLemma
* Lemma maps are flat inflected -> lemma JSON files
* No POS tags
* No metadata
* No nested structures

5.4 Cognate System Requirements
* Unified COGNATE_MAP
* Tier metadata included
* Tier colors defined globally
* Cognates highlighted green
* Clicking a cognate inserts it into the Master List

5.5 Frequency List Requirements
* NGSL for English
* Uniform JSON structure across languages
* Frequency list words are known

5.6 Master List Requirements
* English-only until Supabase integration is complete
* Stored in Supabase (master_wordlists)
* Frontend shape: { word, english, rank, language, length }
* Backend shape: { lemma, rank, language, is_cognate, project_id }
* Column D displays the master list
* Editing occurs inside the UI

5.7 Project List Requirements
* Tracks lemmas used in the current project
* Stored in Supabase (project_wordlists)
* Displayed in Column C

5.8 Curriculum Violations Panel Requirements
* Replaces popup warnings
* Displays unknown words
* Displays curriculum gaps
* Displays out-of-order vocabulary
* Updates on every input event
* Must never interrupt writing flow

6. UI Requirements

6.1 Top Panel Layout
+-----------------------------------------------------------+
| Column A | Column B | Column C | Column D                 |
| Frequency| Cognates | Project  | Master List              |
+-----------------------------------------------------------+

6.2 Writing Window
+-----------------------------------------------------------+
|                 Highlighted Writing Window                |
|                 (textarea + display layer)                |
+-----------------------------------------------------------+

6.3 Violations Panel
+-----------------------------------------------------------+
| Curriculum Violations (scrollable)                       |
+-----------------------------------------------------------+

7. Architecture Overview

7.1 System Diagram
+------------------+       fetch()        +------------------+
|   Frontend       |  ----------------->  |    Backend       |
|  public/app.js   |                     |  src/server.js   |
+------------------+                     +------------------+
        ^                                           v
        |                                           |
        |         Supabase JavaScript Client        |
        |                                           |
        +------------------- Supabase --------------+
                        (projects, wordlists,
                         master lists, cognates)

7.2 Frontend Responsibilities
* Tokenization
* Lemma lookup
* Highlighting
* Rendering lists
* Rendering violations
* Building save payloads
* Normalizing lemmas

7.3 Backend Responsibilities
* Serve static files
* Serve JSON lists
* Provide REST API endpoints
* Map frontend -> Supabase shapes
* Validate save and load requests

7.4 Supabase Responsibilities
* Persistent storage
* Unique constraints
* Row-level security
* Returning inserted rows via select

8. Data Flow Overview

8.1 Save Pipeline Diagram
Frontend saveEverything()
v
POST /api/projects/save
v (returns projectId)
POST /api/projects/wordlist/save (with projectId)
v
Supabase stores rows

8.2 Load Pipeline Diagram
Frontend loadProject(projectId)
v
GET /api/projects/load/:id
v
GET /api/projects/wordlist/load/:id
v
Frontend reconstructs UI

9. Phase Overview

Phase 1 - English-Only Rebuild
* Tokenizer
* Lemma map
* Frequency list
* Highlighting
* Project list
* Violations panel
* Master list (local only)

Phase 2 - Supabase Integration
* Projects table
* Project wordlists table
* Master wordlists table
* Save and load pipeline
* ID propagation
* select after inserts

Phase 3 - Curriculum Modeling
* Out-of-order detection
* Curriculum warnings
* Master list editing
* Cognate insertion rules

Phase 4 - Multiple Curriculum Support
* Multiple master lists
* Comparison tools
* Cross-language master list

Phase 5 - Teacher Workflow Enhancements
* Difficulty metrics
* Coverage metrics
* Export options

Phase 6 - Optional Enhancements
* Collaboration
* Versioning
* Mobile layout
* PWA

10. Constraints
* No frameworks
* No student-facing features
* No autosave
* API routes must be registered before static middleware
* Master list remains English-only until Supabase is stable
* All normalization uses NFD + diacritic stripping

11. Where to Put Updates
* New vocabulary rules -> PRD
* New architecture decisions -> Architecture Overview
* New phase sequencing -> Roadmap
* New linguistic data sources -> Data Sources
* New technical decisions -> Decisions Log
* New feature behavior -> Detailed Feature Specifications
* New terminology -> Glossary
* New immediate tasks -> Next Steps

12. How Copilot Should Interpret This PRD
* Treat this PRD as authoritative
* Follow teacher-only scope
* Follow highlighting priority rules
* Follow phase ordering
* Follow architecture boundaries
* Avoid redesigning features
* Avoid adding student-facing functionality
* Avoid changing vocabulary rules
* Avoid changing UI layout

13. Summary
This PRD defines the complete product vision, scope, architecture, and constraints for WordList Writer. It is the single source of truth for all future development.

Documentation Formatting Reminder
All documentation updates must follow this formatting standard:
* Use plain text section titles (for example: 2. Product Overview)
* Use asterisks (*) for bullet points
* Do not insert blank lines inside bullet lists
* Use ASCII-only characters (no Unicode hyphens, arrows, or punctuation)
* Avoid Markdown headings (#) unless verified safe
* Avoid fenced code blocks unless absolutely necessary
* Keep numbered workflows in Step format (Step 1, Step 2, Step 3) to prevent GitHub auto-renumbering
This ensures consistent rendering across GitHub and prevents formatting breakage.
