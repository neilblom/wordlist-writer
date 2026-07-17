Master_List_Workflow — WordList Writer
Version: 2026-07-17
Status: Authoritative Workflow Guide

Overview
This document explains how to use the Master List (Column D) inside WordList Writer. The Master List is the core of the curriculum system. It determines which words are allowed, which are unknown, which are out-of-order, and how the writing window highlights vocabulary. This workflow guide is designed for teachers and for future development sessions.

1. Purpose of the Master List
The Master List is a pedagogical sequence of lemmas. It defines the order in which vocabulary should appear in beginner-level texts. It is not alphabetical and not frequency-ordered. It is a curriculum.

The Master List supports:
* curriculum alignment
* controlled text creation
* cross-language vocabulary modeling
* cognate scaffolding
* frequency awareness
* project-level diagnostics

2. Where the Master List Appears
Column D in the top panel displays the Master List.

Each row shows:
* rank
* word (lemma)
* language
* cognate flag
* cognates object
* edit button
* delete button

The writing window uses this list to determine highlight colors.

3. How Highlighting Uses the Master List
Highlight priority:
* Green = cognate
* Black = known (frequency list or master list)
* Red = unknown (not in master list)

Curriculum-order violations occur when:
* a lemma appears before its assigned rank
* a lemma is not in the master list at all

Violations appear in the Violations Panel.

4. Adding Lemmas to the Master List
Teachers can add lemmas directly inside Column D.

Supported actions:
* add new lemma
* insert lemma at specific rank
* reorder lemmas (move up/down)
* add cross-language equivalents
* update cognate flags
* update frequency metadata

All changes update:
* masterList array
* masterSet (normalized)
* highlight pipeline
* tooltip metadata
* project list
* Supabase storage (Phase 2)

5. Cognate Insertion Workflow
When a teacher clicks a cognate in Column B:
Step 1: All matching tokens in the writing window highlight green  
Step 2: The cognate is inserted into the Master List  
Step 3: The correct language is assigned  
Step 4: The Master List re-renders  
Step 5: Highlighting updates immediately  

This workflow supports discovery → selection → curriculum building.

6. Typed Word Insertion Workflow
When a teacher types a new English lemma in the writing window:
Step 1: The lemma is normalized  
Step 2: If not in the Master List, it is marked red  
Step 3: Teacher may choose to add it to the Master List  
Step 4: It is inserted after the last lemma appearing in the story  
Step 5: Highlighting updates immediately  

This supports dynamic curriculum development.

7. Curriculum-Order Logic
The Master List defines the allowed sequence.

Example:
* Rank 1: the
* Rank 2: and
* Rank 3: man
* Rank 4: woman

If “woman” appears before “man,” the Violations Panel shows:
* Out-of-order vocabulary

If “child” appears and is not in the list:
* Unknown word

8. Cross-Language Workflow (Future Phases)
The Master List will eventually support:
* english
* spanish
* latin
* greek

Each lemma may include:
* cognates object
* frequency metadata
* tier metadata

Cross-language equivalents allow teachers to build unified curricula.

Example:
{
  word: "and",
  language: "english",
  cognates: {
    spanish: "y",
    latin: "et",
    greek: "και"
  }
}

9. Saving and Loading the Master List
Save Workflow:
Step 1: Teacher clicks “Save Project”  
Step 2: Master List is mapped word → lemma  
Step 3: Rows are inserted into Supabase  
Step 4: Old rows are deleted  
Step 5: .select() returns inserted rows  

Load Workflow:
Step 1: Teacher clicks “Load Project”  
Step 2: Rows are loaded from Supabase  
Step 3: lemma → word mapping applied  
Step 4: Column D re-renders  
Step 5: Highlight pipeline updates  

10. Best Practices for Teachers
* Keep the Master List small and focused  
* Add lemmas only when pedagogically necessary  
* Use cognates to scaffold new vocabulary  
* Check the Violations Panel frequently  
* Maintain consistent ordering  
* Use frequency metadata to guide difficulty  
* Build cross-language equivalents gradually  

11. Best Practices for Developers
* Always normalize lemmas before insertion  
* Ensure masterSet stays in sync with masterList  
* Update highlight pipeline after every edit  
* Validate rank continuity  
* Guard against null values in normalizeLemma  
* Ensure saveEverything() sends correct shape  
* Ensure load pipeline maps lemma → word  

12. Summary
The Master List is the backbone of WordList Writer. It defines curriculum order, controls highlighting, supports multilingual expansion, and enables teachers to build pedagogically sequenced texts. This workflow guide ensures consistent usage and prevents regressions in future development phases.

Documentation Formatting Reminder
* Use plain text section titles
* Use asterisks (*) for bullet points
* No blank lines inside bullet lists
* ASCII-only characters
* Avoid Markdown headings (#)
* Avoid fenced code blocks unless necessary
* Use Step format for workflows
