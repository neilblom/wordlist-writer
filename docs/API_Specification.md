API Specification — WordList Writer
Version: 2026-07-20
Status: Authoritative Backend API Guide

Overview
This document describes the backend API routes for WordList Writer. It defines paths, methods, request and response formats, and error rules. It is backend-only and is designed to keep the server interface stable as new features are added, including cognate management, dictionary profiles, merged dictionary rebuilding, and multilingual expansion.

1. Conventions
General rules:
* all API routes are prefixed with /api
* all requests and responses use JSON
* all responses include success flag
* errors return success: false and error message
* controllers must validate inputs before calling Supabase
* frontend must not assume implicit defaults
* all save routes must return updated projectId when applicable
* Supabase v2 requires .select() after inserts

2. Project Routes

2.1 Save Project
Path:
* POST /api/project/save
Purpose:
* save project metadata, dictionary profile, and text
Request body:
* projectId: string or null
* language: string
* title: string
* text: string
* dictionaryProfile: string
Response:
* success: boolean
* projectId: string
* message: string
Error rules:
* missing language returns error
* missing text returns error
* invalid dictionaryProfile returns error
* Supabase failure returns error

2.2 Load Project
Path:
* POST /api/project/load
Purpose:
* load project metadata and text
Request body:
* projectId: string
Response:
* success: boolean
* projectId: string
* language: string
* title: string
* text: string
* dictionaryProfile: string
Error rules:
* missing projectId returns error
* unknown projectId returns error
* Supabase failure returns error

3. Project Wordlist Routes

3.1 Save Project Wordlist
Path:
* POST /api/project/wordlist/save
Purpose:
* save normalized project wordlist
Request body:
* projectId: string
* projectWordlist: array of strings
Response:
* success: boolean
* message: string
Error rules:
* missing projectId returns error
* projectWordlist must be array of plain strings
* Supabase failure returns error

3.2 Load Project Wordlist
Path:
* POST /api/project/wordlist/load
Purpose:
* load normalized project wordlist
Request body:
* projectId: string
Response:
* success: boolean
* projectWordlist: array of strings
Error rules:
* missing projectId returns error
* unknown projectId returns error
* Supabase failure returns error

4. Master List Routes

4.1 Save Master List
Path:
* POST /api/master/save
Purpose:
* save master list for a project
Request body:
* projectId: string
* masterList: array of strings
Response:
* success: boolean
* message: string
Error rules:
* missing projectId returns error
* masterList must be array of plain strings
* Supabase failure returns error

4.2 Load Master List
Path:
* POST /api/master/load
Purpose:
* load master list for a project
Request body:
* projectId: string
Response:
* success: boolean
* masterList: array of strings
Error rules:
* missing projectId returns error
* unknown projectId returns error
* Supabase failure returns error

5. Dictionary Profile Routes

5.1 Save Dictionary Profile
Path:
* POST /api/profile/save
Purpose:
* save dictionary profile for a project
Request body:
* projectId: string
* dictionaryProfile: string
Response:
* success: boolean
* message: string
Error rules:
* missing projectId returns error
* invalid dictionaryProfile returns error
* Supabase failure returns error

5.2 Load Dictionary Profile
Path:
* POST /api/profile/load
Purpose:
* load dictionary profile for a project
Request body:
* projectId: string
Response:
* success: boolean
* dictionaryProfile: string
Error rules:
* missing projectId returns error
* unknown projectId returns error
* Supabase failure returns error

6. Cognate Routes

6.1 Add Pending Cognate
Path:
* POST /api/cognates/pending/add
Purpose:
* add a new pending cognate for a project and profile
Request body:
* projectId: string
* profile: string
* baseLanguage: string
* lemma: string
* cognate: string
* tier: string
Response:
* success: boolean
* message: string
Error rules:
* missing projectId returns error
* missing lemma or cognate returns error
* invalid profile returns error
* Supabase failure returns error

6.2 Edit Pending Cognate
Path:
* POST /api/cognates/pending/edit
Purpose:
* edit an existing pending cognate
Request body:
* projectId: string
* profile: string
* baseLanguage: string
* lemma: string
* cognate: string
* tier: string
Response:
* success: boolean
* message: string
Error rules:
* missing projectId returns error
* missing lemma returns error
* invalid profile returns error
* Supabase failure returns error

6.3 Publish Cognates
Path:
* POST /api/cognates/publish
Purpose:
* merge pending cognates into official cognates
Request body:
* projectId: string
* profile: string
* baseLanguage: string
Response:
* success: boolean
* message: string
Error rules:
* missing projectId returns error
* invalid profile returns error
* Supabase failure returns error

6.4 Load Cognates (Official + Pending)
Path:
* POST /api/cognates/load
Purpose:
* load official and pending cognates for a project
Request body:
* projectId: string
Response:
* success: boolean
* official: array of objects
* pending: array of objects
Error rules:
* missing projectId returns error
* Supabase failure returns error

7. Cross-Language Master Routes

7.1 Save Cross-Language Master
Path:
* POST /api/crossmaster/save
Purpose:
* save cross-language master list entries
Request body:
* projectId: string
* entries: array of objects
Each entry:
* baseLanguage: string
* lemma: string
* spanish: string or null
* latin: string or null
* greek: string or null
Response:
* success: boolean
* message: string
Error rules:
* missing projectId returns error
* entries must be well-formed objects
* Supabase failure returns error

7.2 Load Cross-Language Master
Path:
* POST /api/crossmaster/load
Purpose:
* load cross-language master list entries
Request body:
* projectId: string
Response:
* success: boolean
* entries: array of objects
Error rules:
* missing projectId returns error
* unknown projectId returns error
* Supabase failure returns error

8. Error Response Format
All error responses follow:
* success: false
* error: string
* details: optional object
Frontend must:
* check success flag
* handle error messages gracefully
* avoid assuming partial success

9. Workflows Summary

SaveEverything API Workflow:
Step 1: frontend calls /api/project/save with metadata and text  
Step 2: backend saves project and returns projectId  
Step 3: frontend calls /api/profile/save  
Step 4: frontend calls /api/project/wordlist/save  
Step 5: frontend calls /api/master/save  
Step 6: frontend calls /api/cognates/pending/add or edit if needed  
Step 7: frontend updates project-id-input with returned projectId

LoadEverything API Workflow:
Step 1: frontend calls /api/project/load  
Step 2: backend returns project metadata, text, dictionaryProfile  
Step 3: frontend calls /api/profile/load  
Step 4: frontend calls /api/project/wordlist/load  
Step 5: frontend calls /api/master/load  
Step 6: frontend calls /api/cognates/load  
Step 7: frontend rebuilds merged dictionary  
Step 8: highlight pipeline runs automatically

Publish Cognates API Workflow:
Step 1: frontend calls /api/cognates/publish  
Step 2: backend merges pending → official  
Step 3: backend clears pending table  
Step 4: backend returns success  
Step 5: frontend reloads cognate data  
Step 6: frontend rebuilds merged dictionary  
Step 7: highlight pipeline runs automatically

10. Summary
This API specification defines the backend interface for WordList Writer. It covers project save/load, master list operations, cognate management, dictionary profiles, merged dictionary rebuilding, and cross-language master data. It is backend-only and is intended to keep the server contract stable as the system evolves.
