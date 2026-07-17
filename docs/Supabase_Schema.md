Supabase Schema Reference — WordList Writer
Version: 2026-07-17
Status: Authoritative Source of Truth

Overview
This document defines the complete Supabase schema for WordList Writer. It is the authoritative reference for all tables, fields, relationships, constraints, and frontend ↔ backend mappings. This schema supports project storage, project wordlists, master lists, and cross-language vocabulary relationships.

1. Table: projects
Purpose:
* Stores metadata for each writing project

Fields:
* id (uuid, primary key)
* name (text)
* language (text)
* created_at (timestamp with time zone, default now())
* updated_at (timestamp with time zone)
* content (text) — raw project text
* notes (text) — optional teacher notes

Constraints:
* id must be non-null
* name required
* language required

Frontend Mapping:
* currentProjectId maps to id
* textarea content maps to content

2. Table: project_wordlists
Purpose:
* Stores normalized lemmas used in a specific project

Fields:
* id (uuid, primary key)
* project_id (uuid, foreign key → projects.id)
* lemma (text)
* language (text)
* is_cognate (boolean)
* created_at (timestamp with time zone, default now())

Constraints:
* project_id required
* lemma required
* (project_id, lemma) should be unique

Frontend Mapping:
Save:
* word → normalizeLemma(word) → lemma
Load:
* lemma → word (frontend uses lemma as word)

3. Table: master_wordlists
Purpose:
* Stores the curriculum sequence for a project

Fields:
* id (uuid, primary key)
* project_id (uuid, foreign key → projects.id)
* lemma (text)
* rank (integer)
* language (text)
* is_cognate (boolean)
* created_at (timestamp with time zone, default now())

Constraints:
* project_id required
* lemma required
* rank required
* (project_id, rank) unique
* (project_id, lemma) unique

Frontend Mapping:
Save:
* word → lemma
* rank preserved
Load:
* lemma → word
* english = lemma
* length = lemma.length

4. Table: cross_language_master
Purpose:
* Stores cross-language equivalents for English lemmas

Fields:
* id (uuid, primary key)
* english (text)
* spanish (text)
* latin (text)
* greek (text)
* cognate_flags (jsonb)
* frequency_ranks (jsonb)
* created_at (timestamp with time zone, default now())

Constraints:
* english required
* All other fields optional
* english unique

Frontend Mapping:
* Column D uses english as anchor
* cognate_flags used for tier-aware highlighting
* frequency_ranks used for tooltip metadata

5. Relationships
projects
* One-to-many → project_wordlists
* One-to-many → master_wordlists

project_wordlists
* Many-to-one → projects

master_wordlists
* Many-to-one → projects

cross_language_master
* Independent table
* No foreign keys
* Used for multilingual expansion

6. Required Indexes
projects
* primary key (id)

project_wordlists
* index on project_id
* unique (project_id, lemma)

master_wordlists
* index on project_id
* unique (project_id, rank)
* unique (project_id, lemma)

cross_language_master
* unique (english)

7. Save/Load Mapping Rules
Save Project
* Save project metadata
* Save project text
* Return id via .select()

Save Wordlist
* Delete old rows for project_id
* Insert new rows
* Use .select() to return inserted rows

Save Master List
* Delete old rows for project_id
* Insert new rows
* Map word → lemma
* Use .select() to return inserted rows

Load Project
* Load project metadata
* Load project_wordlists
* Load master_wordlists
* Convert lemma → word for UI

8. Normalization Requirements
All lemmas stored in Supabase must be normalized:
* lowercase
* NFD Unicode normalization
* diacritic stripping
* punctuation removal

Normalization collisions must be treated as duplicates.

9. Frequency List Ingestion (Supabase)
Fields:
* id (uuid)
* language_id (text)
* lemma (text)
* rank (integer)
* source (text)

Constraints:
* unique (language_id, lemma, source)
* rank must be continuous
* lemma must be normalized

Validation:
* duplicate detection
* rank continuity
* normalization collisions
* file integrity

10. Error Handling Requirements
Missing projectId:
* Reject save
* Log error
* Frontend must set currentProjectId before save

Malformed lemma:
* Reject row
* Frontend must send valid word or english

Rank conflicts:
* Reject row
* Log conflict

11. Schema Change Policy
All schema changes must:
* be documented in /docs/Decisions.md
* include migration steps
* include frontend mapping updates
* include API spec updates

12. Summary
This schema defines all persistent data structures for WordList Writer. It supports project storage, curriculum sequencing, multilingual expansion, cognate metadata, frequency metadata, and cross-language vocabulary modeling. It is the authoritative reference for all backend development and must be kept up to date as new features are added.

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
