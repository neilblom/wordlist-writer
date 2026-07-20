Setup_Supabase.md — WordList Writer
Version: 2026-07-20
Status: Authoritative Setup Guide

Overview
This guide explains how to set up Supabase for WordList Writer. It merges the original setup instructions with updated backend architecture rules, July 19 fixes, project ID rules, master list rules, and Supabase v2 behaviors. Follow these steps exactly to ensure the backend save/load pipeline works correctly.

1. Create Supabase Project
Step 1: go to https://supabase.com
Step 2: click New Project
Step 3: choose a name (e.g., wordlist-writer)
Step 4: choose a password
Step 5: choose region
Step 6: wait for project to initialize
You now have:
* a Postgres database
* REST API endpoints
* Supabase dashboard

2. Retrieve API Keys
In Supabase dashboard:
* go to Project Settings
* go to API
Copy:
* anon key
* service_role key
* project URL
These keys are required for backend integration.

3. Add Environment Variables
In your local environment or Render:
SUPABASE_URL=<your-project-url>
SUPABASE_ANON_KEY=<your-anon-key>
SUPABASE_SERVICE_ROLE_KEY=<your-service-role-key>
These must be available to your Node.js server.

4. Create Tables
Go to SQL Editor → New Query. Run the following SQL blocks.

4.1 Table: projects
create table projects (
id uuid primary key default gen_random_uuid(),
name text not null,
language text not null,
content text,
notes text,
created_at timestamptz default now(),
updated_at timestamptz default now()
);

Code

4.2 Table: project_wordlists
create table project_wordlists (
id uuid primary key default gen_random_uuid(),
project_id uuid references projects(id) on delete cascade,
lemma text not null,
language text not null,
is_cognate boolean default false,
created_at timestamptz default now(),
unique (project_id, lemma)
);

Code

4.3 Table: master_wordlists
create table master_wordlists (
id uuid primary key default gen_random_uuid(),
project_id uuid references projects(id) on delete cascade,
lemma text not null,
rank int not null,
language text not null,
is_cognate boolean default false,
created_at timestamptz default now(),
unique (project_id, rank),
unique (project_id, lemma)
);

Code

4.4 Table: cross_language_master
create table cross_language_master (
id uuid primary key default gen_random_uuid(),
english text not null unique,
spanish text,
latin text,
greek text,
cognate_flags jsonb,
frequency_ranks jsonb,
created_at timestamptz default now()
);

Code

5. Verify Tables
Go to Table Editor and confirm:
* all tables exist
* all columns exist
* constraints are correct

6. Test Connection in Node.js
In src/lib/supabase.js:
import { createClient } from "@supabase/supabase-js";

export const supabase = createClient(
process.env.SUPABASE_URL,
process.env.SUPABASE_SERVICE_ROLE_KEY
);

Code
Test:
const { data, error } = await supabase.from("projects").select("*");
console.log(data, error);

Code
If data prints and error is null, connection works.

7. Test Save Pipeline
Step 1: click New Project in the app
Step 2: type text
Step 3: click Save Project
Backend must:
* save project metadata
* return project id
* save project wordlist
* save master list
Verify in Supabase dashboard:
* projects table has a row
* project_wordlists has rows
* master_wordlists has rows

8. Test Load Pipeline
Step 1: reload page
Step 2: click Load Project
Step 3: select project
Backend must:
* load project metadata
* load project wordlist
* load master list
* restore UI
Highlight pipeline runs automatically.

9. Updated Architecture Rules
Project ID Rules:
* project-id-input must update on create, load, save, and new project
* incorrect project ID causes empty master list loads
Master List Rules:
* masterList must contain plain strings only
* Supabase rejects rows where lemma is undefined
* saveEverything must send valid lemma strings
Supabase v2 Rules:
* .select() required to retrieve inserted rows
* inserts return minimal by default
Static Middleware Rule:
* API routes must be registered before express.static()

10. Common Pitfalls
Missing projectId:
* fix: ensure currentProjectId is set before save
Null lemma:
* fix: normalizeLemma must guard against null
Wrong mapping:
* fix: save uses lemma → lemma
* fix: load assigns masterList directly from returned data
Static middleware shadowing API:
* fix: register API routes before express.static()

11. Summary
You now have:
* Supabase project created
* all tables defined
* environment variables set
* Node.js connected
* save/load pipeline verified
This setup is required before Phase 2 multilingual expansion.
