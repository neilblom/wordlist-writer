Supabase Schema Reference — WordList Writer
Version: 2026-07-20
Status: Authoritative Source of Truth

Overview
This document defines the complete Supabase schema for WordList Writer. It is the authoritative reference for all tables, fields, relationships, constraints, and frontend ↔ backend mappings. This schema supports project storage, project wordlists, master lists, cognate dictionaries, dictionary profiles, and cross-language vocabulary relationships. All tables must remain normalized, restart-proof, and aligned with the July 19 architecture.

1. Table: projects
Purpose:
* stores metadata for each writing project

Fields:
* id (uuid, primary key)
* name (text)
* language (text)
* dictionary_profile (text)
* content (text)
* notes (text)
* created_at (timestamp with time zone, default now())
* updated_at (timestamp with time zone)

Constraints:
* id must be non-null
* name required
* language required
* dictionary_profile required

Frontend Mapping:
* currentProjectId maps to id
* writing window content maps to content
* dictionaryProfile dropdown maps to dictionary_profile

2. Table: project_wordlists
Purpose:
* stores normalized lemmas used in a specific project

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

4. Table: cognates_official
Purpose:
* stores published cognate entries for dictionary profiles

Fields:
* id (uuid, primary key)
* project_id (uuid)
* profile (text)
* base_language (text)
* lemma (text)
* cognate (text)
* tier (text)
* created_at (timestamp with time zone, default now())

Constraints:
* lemma required
* cognate required
* profile required
* (project_id, profile, lemma) unique

Frontend Mapping:
* used for highlight pipeline
* used for alphabetical dictionary
* used for detected cognates

5. Table: cognates_pending
Purpose:
* stores newly added or edited cognates awaiting publication

Fields:
* id (uuid, primary key)
* project_id (uuid)
* profile (text)
* base_language (text)
* lemma (text)
* cognate (text)
* tier (text)
* created_at (timestamp with time zone, default now())

Constraints:
* lemma required
* cognate required
* profile required
* (project_id, profile, lemma) unique

Frontend Mapping:
* used for add/edit workflows
* merged into official on publish
* never used directly for highlight

6. Table: dictionary_profiles
Purpose:
* stores dictionary profile selection per project

Fields:
* id (uuid, primary key)
* project_id (uuid, foreign key → projects.id)
* profile (text)
* created_at (timestamp with time zone, default now())

Constraints:
* project_id required
* profile required
* project_id unique

Frontend Mapping:
* dictionaryProfile dropdown maps to profile
* profile determines cognate filtering

7. Table: cross_language_master
Purpose:
* stores cross-language equivalents for English lemmas

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
* english unique

Frontend Mapping:
* Column D uses english as anchor
* cognate_flags used for tier-aware highlighting
* frequency_ranks used for tooltip metadata

8. Relationships
projects
* one-to-many → project_wordlists
* one-to-many → master_wordlists
* one-to-one → dictionary_profiles
* one-to-many → cognates_official
* one-to-many → cognates_pending

project_wordlists
* many-to-one → projects

master_wordlists
* many-to-one → projects

cognates_official
* many-to-one → projects

cognates_pending
* many-to-one → projects

cross_language_master
* independent table
* no foreign keys

9. Required Indexes
projects
* primary key (id)

project_wordlists
* index on project_id
* unique (project_id, lemma)

master_wordlists
* index on project_id
* unique (project_id, rank)
* unique (project_id, lemma)

cognates_official
* index on project_id
* unique (project_id, profile, lemma)

cognates_pending
* index on project_id
* unique (project_id, profile, lemma)

dictionary_profiles
* unique (project_id)

cross_language_master
* unique (english)

10. Save/Load Mapping Rules
Save Project
Step 1: save project metadata
Step 2: save dictionary profile
Step 3: save project text
Step 4: return id via .select()

Save Wordlist
Step 1: delete old rows for project_id
Step 2: insert new rows
Step 3: return inserted rows

Save Master List
Step 1: delete old rows for project_id
Step 2: insert new rows
Step 3: return inserted rows

Save Cognates
Step 1: write to cognates_pending
Step 2: publish merges pending → official
Step 3: clear pending

Load Project
Step 1: load project metadata
Step 2: load dictionary profile
Step 3: load project_wordlists
Step 4: load master_wordlists
Step 5: load cognates_official
Step 6: load cognates_pending
Step 7: build merged dictionary

11. Normalization Requirements
All lemmas stored in Supabase must be normalized:
* lowercase
* NFD Unicode normalization
* diacritic stripping
* punctuation removal
Normalization collisions must be treated as duplicates.

12. Cognate Merging Rules
Merged dictionary rules:
* pending entries override official entries
* merged dictionary rebuilt after publish
* merged dictionary used for highlight pipeline
* merged dictionary used for hybrid cognate window
* merged dictionary used for alphabetical dictionary

13. Error Handling Requirements
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

Profile conflicts:
* reject invalid profile
* reject missing profile

14. Schema Change Policy
All schema changes must:
* be documented in /docs/Decisions.md
* include migration steps
* include frontend mapping updates
* include API spec updates

15. Summary
This schema defines all persistent data structures for WordList Writer. It supports project storage, curriculum sequencing, multilingual expansion, cognate metadata, dictionary profiles, frequency metadata, and cross-language vocabulary modeling. It is the authoritative reference for all backend development and must be kept up to date as new features are added.
