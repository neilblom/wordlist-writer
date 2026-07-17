Frontend Architecture
Version: 2026-07-17
Status: Authoritative Frontend Guide

Overview
This document explains the frontend architecture of WordList Writer. It describes how the UI is structured, how the text analysis pipeline connects to the interface, and how rendering and updates work. It is designed for future development sessions and for debugging UI-related issues.

1. High-Level Structure
The frontend is a single-page application built with plain HTML, CSS, and JavaScript. There is no framework. The app is divided into:
* top panel with four columns
* bottom panel with writing window
* violations panel
* control buttons such as save and load

The frontend relies on:
* static JSON data loaded from public folders
* in-memory data structures for frequency, lemmas, cognates, and master list
* DOM manipulation for rendering and updates

2. Main UI Regions
The main UI regions are:
* Column A: Frequency List
* Column B: Cognates
* Column C: Project Word List
* Column D: Master List
* Writing Window: text input and highlight overlay
* Violations Panel: curriculum diagnostics

Each region has its own render function and update logic.

3. Data Flow Overview
Frontend data flow:
* load static JSON data at startup
* initialize in-memory structures (frequencyList, lemmaMap, cognateMap, masterList, masterSet, projectListSet)
* attach event listeners to writing window and UI controls
* on text change, run analysis pipeline
* update highlight overlay
* update project list
* update violations panel
* update columns as needed

4. Startup Sequence
On page load:
* load frequency JSON from public/frequency
* load lemma JSON from public/lemmas
* load cognate JSON from public/cognates
* initialize masterList and masterSet
* render initial UI for all columns
* attach event listeners:
  * input event on writing window
  * click events on frequency items
  * click events on cognate items
  * click events on master list controls
  * click events on save and load buttons

5. Writing Window Architecture
The writing window consists of:
* a textarea or contenteditable element for raw text
* an overlay layer for highlights

Workflow:
* teacher types text
* input event triggers analysis pipeline
* tokens are generated
* highlight classes are applied
* overlay is updated to reflect colors and markers

Highlight colors:
* green: cognate
* black: known
* red: unknown

6. Column A: Frequency List
Column A displays frequency data for the active language.

Rendering:
* iterate over frequencyList
* render each item with rank and lemma
* attach click handlers to highlight corresponding tokens in the writing window

Interaction:
* clicking a frequency item:
  * finds matching tokens
  * applies temporary highlight
  * may update project list or master list in future phases

7. Column B: Cognates
Column B displays cognates for the active language.

Rendering:
* iterate over cognateMap
* render each cognate with tier information
* attach click handlers to:
  * highlight matching tokens
  * insert cognate into Master List

Interaction:
* clicking a cognate:
  * highlights all matching tokens green
  * inserts lemma into masterList with appropriate language and flags
  * re-renders Column D

8. Column C: Project Word List
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

9. Column D: Master List
Column D displays the curriculum sequence.

Rendering:
* iterate over masterList
* render each row with:
  * rank
  * word
  * language
  * cognate flag
  * edit and delete controls

Interaction:
* add new lemma
* insert lemma at specific rank
* reorder lemmas
* edit cross-language equivalents
* update cognate flags

Changes in Column D:
* update masterList array
* update masterSet
* trigger re-run of highlight pipeline
* update violations panel

10. Violations Panel Architecture
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

11. Event Handling
Key event handlers:
* input on writing window:
  * triggers analysis pipeline
* click on frequency item:
  * highlights tokens
* click on cognate item:
  * highlights tokens
  * inserts into master list
* click on master list controls:
  * add, edit, delete, reorder
* click on save button:
  * calls saveEverything
* click on load button:
  * calls loadProject

12. Rendering Strategy
Rendering uses:
* direct DOM manipulation
* innerHTML updates for lists
* class-based styling for highlights
* minimal re-rendering to maintain performance

Guidelines:
* avoid full-page re-renders
* update only affected regions
* keep highlight overlay in sync with text

13. Integration With Analysis Pipeline
The frontend architecture is tightly coupled to the analysis pipeline.

Pipeline:
* tokenizer
* normalizeLemma
* cognate lookup
* frequency lookup
* master list lookup
* violation detection

Frontend responsibilities:
* call pipeline on every text change
* apply results to:
  * writing window overlay
  * project list
  * violations panel
  * master list interactions

14. Adding New UI Features
When adding new UI features:
* follow existing render and update patterns
* keep data flow simple and predictable
* avoid mixing analysis logic with rendering logic
* update Developer_Workflow.md and Detailed_Feature_Specifications.md
* ensure new features do not break highlight pipeline or curriculum logic

15. Summary
This frontend architecture document explains how the UI is structured, how data flows through the interface, how events and rendering work, and how the analysis pipeline connects to the visual components. It is essential for debugging UI issues and for safely extending the interface in future development phases.

Documentation Formatting Reminder
* Use plain text section titles
* Use asterisks (*) for bullet points
* No blank lines inside bullet lists
* ASCII-only characters
* Avoid Markdown headings
* Avoid fenced code blocks unless necessary
* Use Step format for workflows
