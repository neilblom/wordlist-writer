API Specification — WordList Writer
Version: 2026-07-17
Status: Authoritative Source of Truth

Overview
This document defines all backend API routes for WordList Writer. It specifies URLs, methods, request/response shapes, ID propagation rules, and frontend ↔ backend mappings. It is the authoritative contract between frontend, backend, and Supabase.

1. Conventions
Base URL:
* /api

Content Type:
* application/json

ID Handling:
* currentProjectId must be set before save
* All save routes return inserted rows via .select()

Error Format:
* { error: string }

2. Health Check
Route:
* GET /api/health

Request:
* No body

Response:
* 200 OK
* { status: "ok" }

Purpose:
* Verify server is running

3. Project Routes

3.1 Create/Save Project
Route:
* POST /api/projects/save

Request Body:
* {
  "id": string | null,
  "name": string,
  "language": string,
  "content": string,
  "notes": string | null
}

Behavior:
* If id is provided:
  * Update existing project
* If id is null:
  * Generate new uuid
* Save project to projects table
* Return full row via .select()

Response:
* 200 OK
* {
  "id": string,
  "name": string,
  "language": string,
  "content": string,
  "notes": string | null,
  "created_at": string,
  "updated_at": string
}

Frontend Requirements:
* new-project-btn sets currentProjectId = crypto.randomUUID()
* load-project-btn sets currentProjectId = projectId
* saveEverything() must capture returned id and store in currentProjectId

3.2 Load Project
Route:
* GET /api/projects/load/:id

Request:
* URL param :id (projectId)

Response:
* 200 OK
* {
  "id": string,
  "name": string,
  "language": string,
  "content": string,
  "notes": string | null,
  "created_at": string,
  "updated_at": string
}

Behavior:
* Load project metadata and content
* Frontend uses content to populate textarea

4. Project Wordlist Routes

4.1 Save Project Wordlist
Route:
* POST /api/projects/wordlist/save

Request Body:
* {
  "projectId": string,
  "wordlist": [
    {
      "word": string,
      "language": string,
      "isCognate": boolean
    }
  ]
}

Behavior:
* Reject if projectId is missing or null
* Delete existing rows for projectId from project_wordlists
* Insert new rows:
  * lemma = normalizeLemma(word)
  * language = language
  * is_cognate = isCognate
* Use .insert([...]).select() to return inserted rows

Response:
* 200 OK
* {
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

Frontend Requirements:
* saveEverything() must pass { projectId: currentProjectId, wordlist }
* Wordlist built from projectListSet and cognate metadata

4.2 Load Project Wordlist
Route:
* GET /api/projects/wordlist/load/:projectId

Request:
* URL param :projectId

Response:
* 200 OK
* {
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

Frontend Mapping:
* lemma → word
* language preserved
* is_cognate used for highlighting and project list display

5. Master List Routes

5.1 Save Master List
Route:
* POST /api/master/save/:projectId

Request:
* URL param :projectId
* Body:
  * {
    "items": [
      {
        "word": string,
        "english": string,
        "rank": number,
        "language": string,
        "isCognate": boolean
      }
    ]
  }

Behavior:
* Reject if projectId missing or null
* Delete existing rows for projectId from master_wordlists
* Insert new rows:
  * lemma = word || english
  * rank = rank
  * language = language
  * is_cognate = isCognate
  * project_id = projectId
* Use .insert([...]).select() to return inserted rows

Response:
* 200 OK
* {
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

Frontend Requirements:
* Master List items use shape:
  * { word, english, rank, language, length }
* saveEverything() must send items array
* normalizeLemma() must guard against null values

5.2 Load Master List
Route:
* GET /api/master/load/:projectId

Request:
* URL param :projectId

Response:
* 200 OK
* {
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

Frontend Mapping:
* lemma → word
* english = lemma
* length = lemma.length
* rank preserved
* language preserved

6. Frequency List Ingestion Routes (Internal / Admin)

6.1 Ingest Frequency List
Route:
* POST /api/frequency/ingest

Request Body:
* {
  "language": string,
  "source": string,
  "entries": [
    {
      "rank": number,
      "lemma": string
    }
  ]
}

Behavior:
* Perform validation:
  * unique lemmas after normalization
  * rank continuity
  * normalization collisions
  * file integrity
* On validation failure:
  * return error
  * do not insert any rows
* On success:
  * insert rows into frequency_lists table
  * enforce unique(language_id, lemma, source)

Response:
* 200 OK
* {
  "inserted": number,
  "duplicatesRemoved": number,
  "collisions": number,
  "missingRanks": number,
  "malformedEntries": number
}

7. Static Data Routes

7.1 Frequency Lists
Route:
* GET /api/frequency/:language

Response:
* 200 OK
* JSON file contents from public/frequency/<language>.json

7.2 Lemma Maps
Route:
* GET /api/lemmas/:language

Response:
* 200 OK
* JSON file contents from public/lemmas/<language>.json

7.3 Cognate Lists
Route:
* GET /api/cognates/:pair

Response:
* 200 OK
* JSON file contents from public/cognates/<pair>.json

8. Error Cases

Missing projectId
* Status: 400
* Body: { error: "Missing projectId" }

Null id on project save
* Status: 400
* Body: { error: "Project id must not be null" }

Malformed Master List item
* Status: 400
* Body: { error: "Invalid master list item" }

Frequency ingestion validation failure
* Status: 400
* Body: { error: "Frequency list validation failed" }

9. Frontend SaveEverything() Contract
saveEverything() must:
Step 1: Call /api/projects/save with currentProjectId (or null)  
Step 2: Capture returned id and set currentProjectId  
Step 3: Call /api/projects/wordlist/save with { projectId: currentProjectId, wordlist }  
Step 4: Call /api/master/save/:projectId with Master List items  

Any deviation from this order can cause:
* null projectId
* rejected saves
* orphaned wordlists
* mismatched master lists

10. API Change Policy
All API changes must:
* be documented in /docs/Decisions.md
* update /docs/Supabase_Schema.md
* update /docs/Detailed_Feature_Specifications.md
* be reflected in frontend code (saveEverything, loaders, mappers)

Summary
This API specification defines the complete backend contract for WordList Writer. It covers project storage, wordlist persistence, master list management, frequency ingestion, and static data access. It is the authoritative reference for all backend development and must be kept in sync with the Supabase schema and documentation.

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
