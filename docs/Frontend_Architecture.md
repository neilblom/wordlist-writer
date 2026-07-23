Frontend Architecture
Version: 2026-07-23
Status: Authoritative Frontend Guide

Overview
This document explains the frontend architecture of WordList Writer. It merges the original UI structure with the updated single-layer editor, centralized highlight pipeline, tokenizer rules, rendering rules, startup sequence, July 19 fixes, hybrid master list model, and Phase 2 cognate system. WordList Writer is a teacher-facing authoring tool. Students never interact with the application. All UI components, including frequency lists, cognates, master lists, warnings, and project tools, are designed exclusively for teacher use.

High-Level Structure
The frontend is a single-page application built with plain HTML, CSS, and JavaScript. There is no framework. The app is divided into:
* top panel with four columns
* bottom panel with writing window
* violations panel
* control buttons such as save and load
The frontend relies on:
* static JSON data loaded from public folders
* in-memory data structures for frequency, lemmas, cognates, and master list
* DOM manipulation for rendering and updates
The frontend performs all text analysis. The backend stores and retrieves data only.

Main UI Regions
The main UI regions are:
* Column A: Frequency List
* Column B: Cognates
* Column C: Project Word List
* Column D: Master List
* Writing Window: text input and highlight rendering
* Violations Panel: curriculum diagnostics
Each region has its own render function and update logic.

Editor Architecture
The writing window is a single contenteditable element:
* no textarea
* no overlay layer
* no dual-layer rendering
* editor.innerText is the source of truth
* editor.innerHTML is replaced on each highlight pass
The editor must never be mutated after rendering. All DOM changes occur only inside renderHighlightsFast. No post-render DOM mutation is allowed.

Centralized Highlight Pipeline
All highlight operations must enter through requestHighlightUpdate. Rules:
* handleStableInput must never be called directly from event listeners
* requestHighlightUpdate uses a 50ms debounce
* no other function may trigger highlight
* no highlight or tokenizer runs during IME composition
* highlight runs only after IME commits final text
This prevents double rendering, nested spans, and DOM corruption.

IME-Safe Input Handling
IME composition rules:
* compositionstart sets isComposing = true
* compositionend calls requestHighlightUpdate
* change calls requestHighlightUpdate
* no highlight or tokenizer runs during composition
This ensures correct behavior for languages requiring IME.

Data Flow Overview
Frontend data flow:
* load static JSON data at startup
* initialize in-memory structures (frequencyList, lemmaMap, cognateMap, masterList, masterSet, projectListSet, frequencySet)
* attach event listeners to writing window and UI controls
* on text change, call requestHighlightUpdate
* highlight pipeline runs once per change
* update project list
* update violations panel
* update columns as needed

Tokenizer Architecture
tokenizeUnified processes raw text into tokens. Rules:
* must not trigger highlight or autosave
* must not log normalized tokens
* must return objects only for real word tokens
* punctuation and whitespace must remain raw strings
* must preserve spacing
* must preserve punctuation
* must not split alphabetic sequences
The tokenizer produces normalized lemmas used for frequency, cognate, and master list detection.

Rendering Pipeline
renderHighlightsFast converts tokens into HTML spans. Responsibilities:
* wrap each token in a span
* apply cognate underline via class
* apply unknown-word asterisk via CSS pseudo-element
* apply frequency known-word styling
* apply master list styling
* return complete HTML string
Rules:
* must not log tokens
* must not mutate DOM directly outside its return value
* must not call highlight or autosave
* must not produce nested spans
Highlight colors:
* green underline: cognate
* normal: known
* red asterisk: unknown

Startup Sequence
On page load:
* load frequency JSON from public/frequency
* load lemma JSON from public/lemmas
* load cognate JSON from public/cognates
* initialize masterList and masterSet
* initialize frequencySet with normalized lemmas
* render initial UI for all columns
* attach event listeners:
  * input and composition events on writing window
  * click events on frequency items
  * click events on cognate items
  * click events on master list controls
  * click events on save and load buttons
Startup workflow:
Step 1: load language and cognates
Step 2: load project text
Step 3: load master list
Step 4: load project wordlist
Step 5: load violations
Step 6: trigger highlight using requestHighlightUpdate
Step 7: after 75ms, run updateProjectList and checkStoryOrderAgainstMaster
Step 8: display "Load complete"

Column A: Frequency List
Column A displays frequency data for the active language.
Rendering:
* iterate over frequencyList
* render each item with rank and lemma
* attach click handlers to highlight corresponding tokens in the writing window
Interaction:
* clicking a frequency item:
  * finds matching tokens
  * applies temporary highlight
Frequency known-word rules:
* known words must come from NGSL-1K frequency list
* frequencySet contains normalized lemmas
* frequency stats computed inside handleStableInput

Column B: Cognates (Phase 2 Simple Cognate Window)
Column B displays cognates detected directly from normalized tokens.
Rendering:
* detect cognates using normalized lemmas
* render detected cognates in a lightweight list
* attach click handlers to:
  * highlight matching tokens
  * insert English lemma into master list
Interaction:
* clicking a cognate:
  * highlights all matching tokens green
  * inserts English lemma into masterList
  * re-renders Column D
Cognate update rules:
* updateCognates must call requestHighlightUpdate
* updateCognates must not call handleStableInput
* cognate updates must re-render master list after highlight
Phase 2 constraints:
* no dictionary profiles
* no alphabetical dictionary
* no pending or official cognates
* no publish workflow
* no cross-language editing
* no tier-based highlighting
* English lemma only is inserted

Column C: Project Word List
Column C displays all lemmas used in the current text.
Rendering:
* iterate over projectListSet
* render each lemma with:
  * cognate tier
  * frequency tier
  * language
Interaction:
* updates automatically when text changes
* reflects current project vocabulary
* used by teachers to review text difficulty
Project list rules:
* updateProjectList must not trigger highlight
* updateProjectList runs after highlight on startup
* updateProjectList may run immediately during user actions

Column D: Master List
Column D displays the curriculum sequence.
Rendering:
* iterate over masterList
* render each row with:
  * rank
  * word
  * language
Interaction:
* add new lemma
* insert lemma at specific rank
* reorder lemmas
* delete lemmas
Changes in Column D:
* update masterList array
* update masterSet
* may trigger re-run of highlight pipeline via requestHighlightUpdate
* update violations panel
Master list rules:
* masterList contains plain strings
* master list rendering occurs after highlight
* master list updates must not call handleStableInput directly

Violations Panel Architecture
The Violations Panel shows curriculum issues.
Rendering:
* iterate over violations array
* render each violation with type and details
Types:
* unknown word
* out-of-order word
* curriculum gap
Interaction:
* read-only for teachers
* used to guide text revisions and master list edits

Autosave Pipeline
Autosave rules:
* autosave must be debounced using autosaveTimer
* autosave must not run inside input handlers
* autosave pauses during project resets
* autosave resumes after reset completes
Autosave is triggered after highlight and project list update.

Backend Interaction
Frontend communicates with backend through API routes:
* apiSaveProject
* apiLoadProject
* apiSaveMasterList
* apiLoadMasterList
Project ID rules:
* project-id-input must always be updated when a new project is created
* project-id-input must always be updated when a project is loaded
* loadEverything must write currentProjectId into project-id-input
* saveEverything must write currentProjectId into project-id-input
* New Project must write currentProjectId into project-id-input
Incorrect project ID handling causes empty master list loads.

Event Handling
Key event handlers:
* input and composition events on writing window:
  * call requestHighlightUpdate
* click on frequency item:
  * highlights tokens
* click on cognate item:
  * highlights tokens
  * inserts English lemma into master list
* click on master list controls:
  * add, delete, reorder
* click on save button:
  * calls saveEverything
* click on load button:
  * calls loadProject

Rendering Strategy
Rendering uses:
* direct DOM manipulation
* innerHTML updates for lists
* class-based styling for highlights
* minimal re-rendering to maintain performance
Guidelines:
* avoid full-page re-renders
* update only affected regions
* keep editor highlights in sync with text
* never mutate editor DOM after renderHighlightsFast

Workflows Summary
Highlight Workflow:
Step 1: user types or IME commits text
Step 2: event listener calls requestHighlightUpdate
Step 3: requestHighlightUpdate debounces highlight
Step 4: handleStableInput runs once
Step 5: highlights are rendered
Step 6: project list updates
Step 7: violations panel updates
Step 8: frequency stats update
Step 9: autosave timer starts

Startup Workflow:
Step 1: load language and cognates
Step 2: load project text
Step 3: load master list
Step 4: load project wordlist
Step 5: load violations
Step 6: trigger highlight using requestHighlightUpdate
Step 7: after 75ms run updateProjectList and checkStoryOrderAgainstMaster
Step 8: display "Load complete"

Summary
This frontend architecture document explains how the UI is structured, how the single-layer editor works, how data flows through the interface, how events and rendering work, and how the analysis pipeline connects to the visual components. It merges the original design with updated Phase 2 architecture rules, July 19 fixes, and the hybrid master list model. It removes Phase 3 cognate dictionary architecture and reflects the correct Phase 2 behavior: English-only master list, simple cognate window, stable tokenizer behavior, and predictable highlight operations for teacher-facing workflows.
