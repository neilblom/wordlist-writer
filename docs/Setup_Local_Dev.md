Setup_Local_Dev — WordList Writer
Version: 2026-07-17
Status: Authoritative Local Development Guide

Overview
This guide explains how to run WordList Writer locally on your computer. It is beginner-friendly and restart-proof. Follow these steps exactly to restore your development environment after a long break.

1. Install Node.js
Step 1: Go to https://nodejs.org  
Step 2: Download the LTS version  
Step 3: Install normally

Verify installation:
node -v  
npm -v

2. Clone the Repository
If the project is backed up on GitHub:
Step 1: Open terminal  
Step 2: Navigate to your development folder  
Step 3: Run:
git clone <your-repo-url>

If the project is not yet on GitHub:
Step 1: Locate your local folder  
Step 2: Copy it to your development machine

3. Install Dependencies
Navigate into the project folder:
cd wordlist-writer

Install all required packages:
npm install

This installs:
* Express
* Supabase client libraries
* dotenv
* all other dependencies

4. Verify Folder Structure
Your project must contain:
docs/  
public/  
src/  
scripts/  
node_modules/  

If any folder is missing, restore it from GitHub or backups.

5. Environment Variables
Create a file:
.env

Add:
SUPABASE_URL=<your-url>  
SUPABASE_ANON_KEY=<your-anon-key>  
SUPABASE_SERVICE_ROLE_KEY=<your-service-role-key>

If Supabase is not set up yet:
* Leave these blank
* The app will run in offline mode (Phase 1)

6. Start the Local Server
Run:
node src/server.js

If you see:
Server running on port 3000

Then open your browser:
http://localhost:3000

You should see the WordList Writer interface.

7. Verify Static Data Loads
The app must load:
* public/frequency/english_ngsl.json  
* public/lemmas/english.json  
* public/cognates/*  

Check the browser console for:
Loaded frequency list  
Loaded lemma map  
Loaded cognates  

If errors appear:
* Check file paths
* Check JSON formatting
* Check server static configuration

8. Test Highlight Pipeline
In the writing window:
Step 1: Type “man”  
Step 2: Type “information”  
Step 3: Type “información” (if Spanish cognates loaded)

Expected behavior:
* Known words = black  
* Cognates = green  
* Unknown words = red asterisk  
* Violations panel updates  

If highlighting fails:
* Check normalizeLemma  
* Check tokenizer  
* Check masterSet  
* Check frequencyList loading

9. Test Project List
Type a short sentence:
“The man walks.”

Column C should show:
man  
walk  
the  

If not:
* Check updateProjectList  
* Check normalization  
* Check projectListSet

10. Test Master List (Offline Mode)
Add a lemma in Column D:
* Click “Add”
* Type “man”
* Assign rank

Check:
* Highlighting updates  
* Violations panel updates  
* Master list re-renders  

If not:
* Check masterList array  
* Check masterSet  
* Check renderMasterList

11. Test Save/Load (If Supabase Configured)
Click “Save Project.”

Expected:
* Project saved  
* Wordlist saved  
* Master list saved  
* ID returned  

Click “Load Project.”

Expected:
* Text restored  
* Project list restored  
* Master list restored  
* UI restored  

If save fails:
* Check currentProjectId  
* Check environment variables  
* Check API routes  
* Check Supabase connection

12. Common Local Development Pitfalls
Static middleware shadowing API
* Fix: register API routes before express.static()

Missing projectId
* Fix: new-project-btn must set currentProjectId

Null lemma
* Fix: normalizeLemma must guard against null

Wrong mapping
* Save uses word → lemma  
* Load uses lemma → word  

13. Recommended Local Development Workflow
Step 1: Start server  
Step 2: Open browser  
Step 3: Write text  
Step 4: Watch highlight colors  
Step 5: Adjust Master List  
Step 6: Check violations  
Step 7: Save project  
Step 8: Load project  
Step 9: Repeat

14. Summary
This guide explains how to run WordList Writer locally, install dependencies, configure environment variables, start the server, verify pipelines, and test save/load behavior. It is designed for future you when returning after a break.

Documentation Formatting Reminder
* Use plain text section titles
* Use asterisks (*) for bullet points
* No blank lines inside bullet lists
* ASCII-only characters
* Avoid Markdown headings (#)
* Avoid fenced code blocks unless necessary
* Use Step format for workflows
