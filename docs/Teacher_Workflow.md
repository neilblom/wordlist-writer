Teacher_Workflow — WordList Writer
Version: 2026-07-20
Status: Authoritative Teacher Guide

Overview
This document explains how teachers use WordList Writer to create controlled texts for beginner learners. It merges the original teacher workflow with updated architecture rules, highlight pipeline rules, tokenizer rules, rendering rules, dictionary profile system, hybrid cognate window, alphabetical dictionary, cognate merging rules, and July 19 fixes. It focuses on workflow, not code. It is designed for instructors who want to build pedagogically sequenced texts aligned with curriculum, frequency lists, and cognate scaffolding. WordList Writer is a teacher-facing authoring tool. Students never see the interface.

1. Teacher Workflow Philosophy
WordList Writer is designed for instructors who need to:
* write texts
* check vocabulary difficulty
* enforce curriculum order
* scaffold learning with cognates
* track project vocabulary
* build or refine a master vocabulary list
The final output delivered to students is a clean text without highlights. All UI components are teacher-only.

2. The Writing Window
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

3. Column A — Frequency List
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

4. Column B — Cognates (Hybrid Window)
Column B displays cognates filtered by the active dictionary profile. It contains two sections:
* detected cognates (top)
* alphabetical dictionary (bottom)
Teacher uses:
* identify helpful cross-language scaffolding
* click detected cognates to highlight them in the text
* click alphabetical entries to explore cognates
* insert cognates into the Master List automatically
* add new cognates
* edit pending cognates
* publish pending cognates into official dictionary
Best practice:
* use cognates to introduce new concepts gently
* add cognates to the Master List when they support curriculum goals
Rules:
* updateCognates must call requestHighlightUpdate
* updateCognates must not call handleStableInput directly
* alphabetical dictionary must reflect merged dictionary
* dictionaryProfile determines which cognates appear

5. Dictionary Profile Dropdown
The dictionary profile determines which cognates are visible in Column B.
Profiles:
* spanish
* latin
* greek
* merged
Teacher uses:
* choose the cognate set appropriate for learners
* switch profiles to explore different cognate scaffolds
Rules:
* dictionaryProfile is stored per project
* profile affects cognate filtering only
* profile does not change project language
* profile changes must trigger requestHighlightUpdate

6. Column C — Project Word List
Tracks all lemmas used in the current text.
Teacher uses:
* see which words appear in the text
* check cognate tier
* check frequency tier
* identify unknown or out-of-order words
* compare project vocabulary with Master List
Best practice:
* review project vocabulary before finalizing a text
* remove or replace words that are too advanced
Rules:
* updateProjectList must not trigger highlight
* updateProjectList runs after highlight on startup

7. Column D — Master List
Your curriculum sequence.
Teacher uses:
* add new lemmas
* insert lemmas at specific ranks
* reorder lemmas
* add cross-language equivalents
* update cognate flags
Highlighting depends on this list.
Best practice:
* keep the list small and focused
* add lemmas only when pedagogically justified
* maintain consistent ordering
* use cross-language equivalents to build unified curricula
Rules:
* masterList contains plain strings only
* master list updates must trigger requestHighlightUpdate

8. Violations Panel
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

9. Cognate Workflow
Step 1: teacher clicks a cognate in Column B
Step 2: matching tokens highlight green
Step 3: cognate is inserted into Master List
Step 4: Master List re-renders
Step 5: requestHighlightUpdate runs the highlight pipeline
This supports discovery -> selection -> curriculum building.

10. Typed Word Workflow
Step 1: teacher types a new word
Step 2: word is normalized
Step 3: if unknown, it highlights red
Step 4: teacher may add it to Master List
Step 5: it is inserted after last lemma appearing in the story
Step 6: requestHighlightUpdate runs the highlight pipeline
This supports dynamic curriculum development.

11. Cognate Add/Edit/Publish Workflow
Add Workflow:
Step 1: teacher clicks Add Cognate
Step 2: teacher enters lemma, cognate, tier
Step 3: entry saved to pending cognates
Step 4: merged dictionary rebuilt
Step 5: requestHighlightUpdate refreshes highlights

Edit Workflow:
Step 1: teacher clicks Edit on a pending cognate
Step 2: teacher updates fields
Step 3: entry updated in pending cognates
Step 4: merged dictionary rebuilt
Step 5: requestHighlightUpdate refreshes highlights

Publish Workflow:
Step 1: teacher clicks Publish Cognates
Step 2: pending entries merged into official dictionary
Step 3: pending entries cleared
Step 4: merged dictionary rebuilt
Step 5: requestHighlightUpdate refreshes highlights

12. Saving Your Work
When you click Save Project, the app saves:
* project metadata
* dictionary profile
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

13. Loading Your Work
When you click Load Project, the app restores:
* text
* dictionary profile
* project wordlist
* master list
* cognates (official and pending)
* merged dictionary
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

14. Exporting Clean Text (Future Phase)
Teachers will be able to export:
* clean text without highlights
* vocabulary summaries
* curriculum diagnostics
This supports classroom use.

15. Multilingual Workflow (Future Phases)
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

16. Teacher Tips for Effective Use
* write first, analyze second
* use cognates strategically
* keep curriculum simple
* avoid rare words early
* check violations panel often
* build Master List slowly
* use frequency ranks to guide difficulty
* save projects regularly
* review project vocabulary before finalizing

17. Summary
WordList Writer is a teacher-facing authoring tool designed to help instructors create controlled texts aligned with curriculum, frequency lists, cognate scaffolding, and dictionary profiles. This updated workflow merges the original teacher guide with new architecture rules, hybrid cognate window, alphabetical dictionary, cognate merging rules, and July 19 fixes to ensure stable and predictable behavior across the entire system.
