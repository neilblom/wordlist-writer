Teacher_Workflow — WordList Writer
Version: 2026-07-23
Status: Authoritative Teacher Guide

Overview
This document explains how teachers use WordList Writer to create controlled texts for beginner learners. It merges the original teacher workflow with updated Phase 2 architecture rules, highlight pipeline rules, tokenizer rules, rendering rules, hybrid master list model, and July 19 fixes. All Phase 3 dictionary profile, alphabetical dictionary, merged dictionary, and cognate publishing features have been removed. WordList Writer is a teacher-facing authoring tool. Students never see the interface.

Teacher Workflow Philosophy
WordList Writer is designed for instructors who need to:
* write texts
* check vocabulary difficulty
* enforce curriculum order
* scaffold learning with cognates
* track project vocabulary
* build or refine a master vocabulary list
The final output delivered to students is a clean text without highlights. All UI components are teacher-only.

The Writing Window
The writing window is a single contenteditable element. As teachers type:
* words are tokenized and normalized
* lemmas are detected
* cognates highlight green
* known words display normally
* unknown words show a red asterisk
* violations appear in the panel above
Teacher actions:
* type normally
* watch highlight colors
* use violations to guide revisions
* add new lemmas to the Master List when appropriate
Rules:
* highlight updates occur only through requestHighlightUpdate
* no highlight occurs during IME composition

Column A — Frequency List
Shows the most common words in the selected language.
Teacher uses:
* check whether a word is high-frequency
* decide if a word is appropriate for beginners
* click a word to highlight it in the text
* compare frequency rank with curriculum rank
Best practice:
* prefer lower-rank words for early lessons
* avoid rare words unless pedagogically necessary
Rules:
* known words come from NGSL-1K frequency list
* frequencySet contains normalized lemmas

Column B — Cognates (Phase 2 Simple Cognate Window)
Column B displays detected cognates only.
Teacher uses:
* identify helpful cross-language scaffolding
* click detected cognates to highlight them in the text
* insert English lemmas into the Master List automatically
Best practice:
* use cognates to introduce new concepts gently
* add cognates to the Master List when they support curriculum goals
Rules:
* updateCognates must call requestHighlightUpdate
* updateCognates must not call handleStableInput directly
Phase 2 constraints:
* no alphabetical dictionary
* no dictionary profiles
* no pending or official cognates
* no publish workflow

Column C — Project Word List
Tracks all lemmas used in the current text.
Teacher uses:
* see which words appear in the text
* check cognate tier (Phase 2 simplified)
* check frequency tier
* identify unknown or out-of-order words
* compare project vocabulary with Master List
Best practice:
* review project vocabulary before finalizing a text
* remove or replace words that are too advanced
Rules:
* updateProjectList must not trigger highlight
* updateProjectList runs after highlight on startup

Column D — Master List
Your curriculum sequence.
Teacher uses:
* add new lemmas
* insert lemmas at specific ranks
* reorder lemmas
Highlighting depends on this list.
Best practice:
* keep the list small and focused
* add lemmas only when pedagogically justified
* maintain consistent ordering
Rules:
* masterList contains plain strings only
* master list updates must trigger requestHighlightUpdate

Violations Panel
Shows curriculum issues in real time.
Types:
* unknown word
* curriculum gap
* out-of-order vocabulary
Teacher uses:
* identify words that break curriculum sequence
* replace or reorder problematic vocabulary
* add missing lemmas to the Master List
* validate text difficulty
Best practice:
* keep violations panel empty before exporting text
* use violations to refine curriculum
Rules:
* violations must be computed after highlight

Cognate Workflow (Phase 2)
Step 1: teacher clicks a cognate in Column B
Step 2: matching tokens highlight green
Step 3: English lemma is inserted into Master List
Step 4: Master List re-renders
Step 5: requestHighlightUpdate runs the highlight pipeline
This supports discovery → selection → curriculum building.

Typed Word Workflow
Step 1: teacher types a new word
Step 2: word is normalized
Step 3: if unknown, it highlights red
Step 4: teacher may add it to Master List
Step 5: it is inserted after last lemma appearing in the story
Step 6: requestHighlightUpdate runs the highlight pipeline
This supports dynamic curriculum development.

Saving Your Work
When you click Save Project, the app saves:
* project metadata
* project text
* project wordlist
* master list
Teacher uses:
* save progress
* switch between projects
* maintain multiple curricula
Best practice:
* save frequently
* name projects clearly
Rules:
* project-id-input must always be updated
* masterList must be saved as plain strings

Loading Your Work
When you click Load Project, the app restores:
* text
* project wordlist
* master list
* UI state
Teacher uses:
* continue previous work
* compare multiple texts
* build long-term curricula
Best practice:
* load projects before writing new texts
* keep project list organized
Rules:
* masterList assigned directly from Supabase data
* highlight pipeline must run after load

Exporting Clean Text (Future Phase)
Teachers will be able to export:
* clean text without highlights
* vocabulary summaries
* curriculum diagnostics
This supports classroom use.

Multilingual Workflow (Future Phases)
Teachers will be able to:
* switch languages
* load Spanish, Latin, Greek frequency lists
* load multilingual lemma maps
* load multilingual cognates
* build cross-language master lists
Best practice:
* start with English-only
* add languages gradually
* use cognates to bridge languages

Teacher Tips for Effective Use
* write first, analyze second
* use cognates strategically
* keep curriculum simple
* avoid rare words early
* check violations panel often
* build Master List slowly
* use frequency ranks to guide difficulty
* save projects regularly
* review project vocabulary before finalizing

Summary
WordList Writer is a teacher-facing authoring tool designed to help instructors create controlled texts aligned with curriculum, frequency lists, and cognate scaffolding. This updated workflow reflects correct Phase 2 behavior: English-only master list, simple cognate window, stable tokenizer behavior, and predictable highlight operations. All Phase 3 cognate dictionary architecture has been removed to maintain stability.
