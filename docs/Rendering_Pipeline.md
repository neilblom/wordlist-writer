Rendering Pipeline
Version: 2026-07-20
Status: Authoritative Rendering Guide

Overview
This document explains how the rendering pipeline works in WordList Writer. It merges the original rendering design with the updated single-layer editor, centralized highlight pipeline, tokenizer rules, rendering rules, startup sequence rules, and July 19 fixes. It describes how tokens become DOM nodes, how highlight classes are applied, how UI components re-render, and how rendering integrates with the analysis pipeline.

1. Rendering Philosophy
The rendering pipeline is designed to be:
* simple
* predictable
* incremental
* fast
The app avoids full-page re-renders. Only affected regions update after each highlight pass. Rendering must never mutate the DOM after completion.

2. Core Rendering Components
The rendering pipeline updates:
* writing window
* project word list
* master list
* frequency list
* cognate list
* violations panel
Most updates occur in the writing window and project list.

3. Rendering Sequence
Rendering follows this order:
* tokenize text
* normalize lemmas
* run analysis pipeline
* compute highlight classes
* build HTML spans
* replace editor.innerHTML
* update project list
* update violations panel
* update master list if needed
This order must remain stable.

4. Writing Window Rendering
The writing window uses a single contenteditable layer. There is no overlay. Rendering steps:
* read raw text from editor.innerText
* tokenize text
* generate HTML spans
* apply highlight classes
* replace editor.innerHTML with new HTML
Highlight classes:
* cognate-green
* unknown-word
* known-word (implicit)
Rules:
* no nested spans
* no DOM mutation after render
* no tooltip metadata
* whitespace preserved exactly
* punctuation preserved exactly

5. Token Rendering Rules
Each token becomes a span element. Rules:
* preserve original text
* apply class based on analysis
* maintain whitespace between tokens
* maintain punctuation tokens as raw strings
* attach data-lemma attribute
Token types:
* word tokens become objects
* punctuation and whitespace remain raw strings

6. Project List Rendering
Project list rendering steps:
* clear list
* iterate over projectListSet
* render each lemma with metadata
* append to Column C
Metadata includes:
* cognate tier
* frequency tier
* language
Rules:
* updateProjectList must not trigger highlight
* updateProjectList runs after highlight on startup

7. Master List Rendering
Master list rendering steps:
* clear list
* iterate over masterList
* render each row with:
  * rank
  * word
  * language
  * cognate flag
  * edit and delete controls
* append to Column D
Master list changes trigger:
* requestHighlightUpdate
* violations panel update

8. Violations Panel Rendering
Violations panel rendering steps:
* clear panel
* iterate over violations array
* render each violation with type and details
* append to panel
Violation types:
* unknown word
* out-of-order word
* curriculum gap

9. Event-Driven Rendering
Rendering is triggered by:
* requestHighlightUpdate
* click event in frequency list
* click event in cognate list
* edit event in master list
* save and load events
Rules:
* handleStableInput must never be called directly
* requestHighlightUpdate must debounce highlight
* no highlight during IME composition

10. Performance Considerations
Performance rules:
* avoid full re-renders
* avoid deep DOM trees
* keep token spans lightweight
* avoid nested spans
* avoid DOM mutation after render
The pipeline must remain fast enough for real-time typing.

11. Rendering and Analysis Integration
Rendering depends on analysis results. Analysis pipeline provides:
* tokens
* normalized lemmas
* cognate flags
* frequency tiers
* curriculum ranks
* violation list
Rendering pipeline consumes these results and produces HTML.

12. Updated Architecture Rules
Centralized Highlight Pipeline:
* all highlight operations must enter through requestHighlightUpdate
* requestHighlightUpdate uses debounce
* no other function may trigger highlight
* no highlight during IME composition
Startup Highlight Sequence:
* startup triggers highlight once
* project list and order check run after highlight
* recommended delay is 75ms
Startup Frequency Loop Removal:
* triggerFrequencyLoop must not run on startup
* frequency stats computed inside handleStableInput
Debug Logging Rules:
* no normalized token logs
* no lemmaMap prefix logs
* no sample frequency logs
* only "Load complete" allowed on startup

13. Adding New Rendering Features
When adding new rendering features:
* keep DOM structure simple
* avoid nested spans
* update highlight classes consistently
* update Detailed_Feature_Specifications.md
* update Developer_Workflow.md

14. Debugging Rendering Issues
Debugging steps:
* check tokenizer output
* check normalized lemmas
* check highlight classes
* check editor.innerHTML for nested spans
* check project list rendering
* check master list rendering
* check violations panel rendering

15. Summary
This rendering pipeline document explains how tokens become DOM nodes, how highlight classes are applied, how UI components re-render, and how rendering integrates with the analysis pipeline. It merges the original design with updated architecture rules and July 19 fixes. It is essential for debugging UI update issues and safely extending the rendering system.
