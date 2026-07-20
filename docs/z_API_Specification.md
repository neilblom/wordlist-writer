API Specification — WordList Writer
Version: 2026-07-20
Status: Authoritative Source of Truth

Overview
This document defines all backend API routes for WordList Writer. It merges the original API specification with updated backend architecture rules, July 19 fixes, project ID rules, master list rules, and Supabase v2 behaviors. It is the authoritative contract between frontend, backend, and Supabase.

1. Conventions
Base URL:
* /api
Content Type:
* application/json
ID Handling:
* currentProjectId must be set before save
* all save routes must return inserted rows via .select()
Error Format:
* { error: string }

2. Health Check
Route:
* GET /api/health
Request:
* no body
Response:
* { status: "ok" }
Purpose:
* verify server is running

3. Project Routes

3.1 Save Project
Route:
* POST /api/projects/save
Request Body:
{
"id": string | null,
"name": string,
"language": string,
"content": string,
"notes": string | null
}

Code
Behavior:
* if id is provided:
  * update existing project
* if id is null:
  * generate new uuid
* save project to projects table
* return full row via .select()
Response:
{
"id": string,
"name": string,
"language": string,
"content": string,
"notes": string | null,
"created_at": string,
"updated_at": string
}

Code
Frontend Requirements:
* new-project-btn sets currentProjectId = crypto.randomUUID()
* load-project-btn sets currentProjectId = projectId
* saveEverything must capture returned id and store it in currentProjectId

3.2 Load Project
Route:
* GET /api/projects/load/:id
Response:
{
"id": string,
"name": string,
"language": string,
"content": string,
"notes": string | null,
"created_at": string,
"updated_at": string
}

Code
Behavior:
* load project metadata and content
* frontend uses content to populate editor.innerText

4. Project Wordlist Routes

4.1 Save Project Wordlist
Route:
* POST /api/projects/wordlist/save
Request Body:
{
"projectId": string,
"wordlist": [
{
"word": string,
"language": string,
"isCognate": boolean
}
]
}

Code
Behavior:
* reject if projectId missing or null
* delete existing rows for projectId
* insert new rows:
  * lemma = normalizeLemma(word)
  * language = language
  * is_cognate = isCognate
* use .insert([...]).select() to return inserted rows
Response:
{
"rows": [
{
"id": string,
"project_id": string,
"lemma": string,
"language": string,
"is_cognate": boolean,
"created_at": string
}
]
}

Code
Frontend Requirements:
* saveEverything must pass { projectId: currentProjectId, wordlist }
* wordlist built from projectListSet and cognate metadata

4.2 Load Project Wordlist
Route:
* GET /api/projects/wordlist/load/:projectId
Response:
{
"rows": [
{
"id": string,
"project_id": string,
"lemma": string,
"language": string,
"is_cognate": boolean,
"created_at": string
}
]
}

Code
Frontend Mapping:
* lemma → word
* language preserved
* is_cognate used for highlighting and project list display

5. Master List Routes

5.1 Save Master List
Route:
* POST /api/master/save/:projectId
Request Body:
{
"items": [
{
"word": string,
"rank": number,
"language": string,
"isCognate": boolean
}
]
}

Code
Behavior:
* reject if projectId missing or null
* delete existing rows for projectId
* insert new rows:
  * lemma = word
  * rank = rank
  * language = language
  * is_cognate = isCognate
  * project_id = projectId
* use .insert([...]).select() to return inserted rows
Response:
{
"rows": [
{
"id": string,
"project_id": string,
"lemma": string,
"rank": number,
"language": string,
"is_cognate": boolean,
"created_at": string
}
]
}

Code
Frontend Requirements:
* masterList must contain plain strings only
* saveEverything must send items array
* normalizeLemma must guard against null values

5.2 Load Master List
Route:
* GET /api/master/load/:projectId
Response:
{
"rows": [
{
"id": string,
"project_id": string,
"lemma": string,
"rank": number,
"language": string,
"is_cognate": boolean,
"created_at": string
}
]
}

Code
Frontend Mapping:
* lemma → word
* rank preserved
* language preserved

6. Static Data Routes

6.1 Frequency Lists
Route:
* GET /api/frequency/:language
Response:
* JSON file contents from public/frequency/<language>.json

6.2 Lemma Maps
Route:
* GET /api/lemmas/:language
Response:
* JSON file contents from public/lemmas/<language>.json

6.3 Cognate Lists
Route:
* GET /api/cognates/:pair
Response:
* JSON file contents from public/cognates/<pair>.json

7. Error Cases
Missing projectId:
* { error: "Missing projectId" }
Null id on project save:
* { error: "Project id must not be null" }
Malformed master list item:
* { error: "Invalid master list item" }
Frequency ingestion validation failure:
* { error: "Frequency list validation failed" }

8. SaveEverything Contract
saveEverything must:
Step 1: call /api/projects/save with currentProjectId (or null)
Step 2: capture returned id and set currentProjectId
Step 3: call /api/projects/wordlist/save with { projectId: currentProjectId, wordlist }
Step 4: call /api/master/save/:projectId with master list items
Any deviation causes:
* null projectId
* rejected saves
* orphaned wordlists
* mismatched master lists

9. Updated Architecture Rules
Project ID Rules:
* project-id-input must update on create, load, save, and new project
Master List Rules:
* masterList must contain plain strings only
Supabase v2 Rules:
* .select() required for all inserts
Static Middleware Rule:
* API routes must be registered before express.static()

10. API Change Policy
All API changes must:
* be documented in docs/Decisions.md
* update docs/Supabase_Schema.md
* update docs/Detailed_Feature_Specifications.md
* be reflected in frontend save/load code

Summary
This API specification defines the complete backend contract for WordList Writer. It covers project storage, wordlist persistence, master list management, static data access, and updated Supabase v2 behaviors. It is the authoritative reference for backend development and must remain synchronized with the Supabase schema and documentation.
