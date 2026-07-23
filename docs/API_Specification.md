API Specification — WordList Writer
Version: 2026-07-23
Status: Authoritative Backend API Guide

Overview
This document describes the backend API routes for WordList Writer. It defines paths, methods, request and response formats, and error rules. It is backend-only and is designed to keep the server interface stable as new features are added. All Phase 3 features (dictionary profiles, merged dictionary, alphabetical dictionary, pending/official cognates, tier metadata, cross-language master) have been removed. This specification reflects the simplified Phase 2 architecture: English-only master list, simple cognate window, stable tokenizer, stable highlight pipeline, and predictable save/load behavior.

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
* save project metadata and text
Request body:
* projectId: string or null
* language: string
* title: string
* text: string
Response:
* success: boolean
* projectId: string
* message: string
Error rules:
* missing language returns error
* missing text returns error
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

5. Cognate Routes (Phase 2 Simple Cognate Window)
Purpose:
* simple Spanish → English cognate detection
* no pending/official tables
* no dictionary profiles
* no tier metadata
* no alphabetical dictionary

5.1 Load Static Cognates
Path:
* GET /api/cognates/static
Purpose:
* load static Spanish → English cognate list from JSON
Response:
* success: boolean
* cognates: array of objects
Error rules:
* missing file returns error
* malformed JSON returns error

6. Error Response Format
All error responses follow:
* success: false
* error: string
* details: optional object
Frontend must:
* check success flag
* handle error messages gracefully
* avoid assuming partial success

7. Workflows Summary

SaveEverything API Workflow:
Step 1: frontend calls /api/project/save with metadata and text  
Step 2: backend saves project and returns projectId  
Step 3: frontend calls /api/project/wordlist/save  
Step 4: frontend calls /api/master/save  
Step 5: frontend updates project-id-input with returned projectId

LoadEverything API Workflow:
Step 1: frontend calls /api/project/load  
Step 2: backend returns project metadata and text  
Step 3: frontend calls /api/project/wordlist/load  
Step 4: frontend calls /api/master/load  
Step 5: frontend loads static cognates  
Step 6: highlight pipeline runs automatically

8. Summary
This API specification defines the backend interface for WordList Writer. It covers project save/load, master list operations, project wordlist operations, and static cognate loading. It is backend-only and is intended to keep the server contract stable as the system evolves. All Phase 3 dictionary profile and cognate dictionary features have been removed to maintain Phase 2 stability.
