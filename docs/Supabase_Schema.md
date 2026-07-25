Supabase Schema Reference — WordList Writer
Version: 2026-07-23
Status: Authoritative Source of Truth

Overview
This document defines the complete Supabase schema for WordList Writer. It is the authoritative reference for all tables, fields, relationships, constraints, and frontend ↔ backend mappings. This schema supports project storage, project wordlists, and master lists. All tables must remain normalized, restart-proof, and aligned with the July 19 architecture and Phase 2 constraints.

Schema Lock Rule  
The projects table schema is fixed.
Do NOT add, rename, or remove columns unless explicitly instructed.
All backend and frontend fields MUST match the Supabase schema exactly.
Copilot must NOT introduce new fields, rename fields, or “improve” the schema.
Copilot must treat the schema as locked and immutable.

Schema Stability Rule  
The projects table schema is locked and must not change.
Copilot must NOT add, rename, or remove columns unless Neil explicitly instructs it.
All frontend → backend → Supabase fields must match this schema exactly.

Sync Rule  
Every field sent by the frontend must exist in the backend route.
Every field used in the backend route must exist in the Supabase table.
No extra fields. No missing fields. No renamed fields.

1. Table: projects
Purpose:
* stores metadata for each writing project

Fields:
projects table schema (locked):

id          uuid (PK)
title       text
language    text
content     text
created_at  timestamptz
updated_at  timestamptz
user_id     uuid

Constraints:
* id must be non-null
* title required
* language required

Frontend Mapping:
* currentProjectId maps to id
* writing window content maps to content

2. Table: project_wordlists
Purpose:
* stores normalized lemmas used in a specific project

Fields:
* id (uuid, primary key)
* project_id (uuid references projects(id) on delete cascade)
* lemma (text)
* language (text)
* created_at (timestamptz default now())

Constraints:
* project_id required
* lemma required
* (project_id, lemma) unique

Frontend Mapping:
Save:
* word → normalizeLemma(word) → lemma
Load:
* lemma → word (frontend uses lemma as word)

3. Table: master_wordlists
Purpose:
* stores the curriculum sequence for a project

Fields:
* id (uuid, primary key)
* project_id (uuid references projects(id) on delete cascade)
* lemma (text)
* rank (integer)
* language (text)
* created_at (timestamptz default now())

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

4. Relationships
projects
* one-to-many → project_wordlists
* one-to-many → master_wordlists

project_wordlists
* many-to-one → projects

master_wordlists
* many-to-one → projects

5. Required Indexes
projects
* primary key (id)

project_wordlists
* index on project_id
* unique (project_id, lemma)

master_wordlists
* index on project_id
* unique (project_id, rank)
* unique (project_id, lemma)

6. Save/Load Mapping Rules
Save Project
Step 1: save project metadata
Step 2: return id via .select()

Save Wordlist
Step 1: delete old rows for project_id
Step 2: insert new rows
Step 3: return inserted rows

Save Master List
Step 1: delete old rows for project_id
Step 2: insert new rows
Step 3: return inserted rows

Load Project
Step 1: load project metadata
Step 2: load project_wordlists
Step 3: load master_wordlists

7. Normalization Requirements
All lemmas stored in Supabase must be normalized:
* lowercase
* NFD Unicode normalization
* diacritic stripping
* punctuation removal
Normalization collisions must be treated as duplicates.

8. Error Handling Requirements
Missing projectId:
* reject save
* log error
* frontend must set currentProjectId before save

Malformed lemma:
* reject row
* frontend must send valid word or english

Rank conflicts:
* reject row
* log conflict

9. Schema Change Policy
All schema changes must:
* be documented in /docs/Decisions.md
* include migration steps
* include frontend mapping updates
* include API spec updates

10. Summary
This schema defines all persistent data structures for WordList Writer. It supports project storage, curriculum sequencing, and project-level vocabulary tracking. It is the authoritative reference for all backend development and must be kept up to date as new features are added.
