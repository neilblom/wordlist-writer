Rendering Pipeline
Version: 2026-07-23
Status: Authoritative Rendering Guide

Overview
This document explains how the rendering pipeline works in WordList Writer. It merges the original rendering design with the updated single-layer editor, centralized highlight pipeline, tokenizer rules, rendering rules, startup sequence rules, hybrid master list model, July 19 fixes, and the simplified Phase 2 cognate system. All Phase 3 dictionary profile, alphabetical dictionary, and merged dictionary features have been removed. WordList Writer is a teacher-facing authoring tool.

Rendering Philosophy
The rendering pipeline is designed to be:
* simple
* predictable
* incremental
* fast
The app avoids full-page re-renders. Only affected regions update after each highlight pass. Rendering must never mutate the DOM after completion.

Core Rendering Components
The rendering pipeline updates:
* writing window
* project word list
* master list
* frequency list
* cognate window (detected only)
* violations panel
Most updates occur in the writing window and project list.

Rendering Sequence
Rendering follows this order:
* tokenize text
* normalize lemmas
* run analysis pipeline
* compute highlight classes
* build HTML spans
* replace editor.innerHTML
* update project list
* update violations panel
* update cognate detected section
* update master list if needed
This order must remain stable.

Writing Window Rendering
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

Token Rendering Rules
Each token becomes a span element. Rules:
* preserve original text
* apply class based on analysis
* maintain whitespace between tokens
* maintain punctuation tokens as raw strings
* attach data-lemma attribute
Token types:
* word tokens become objects
* punctuation and whitespace remain raw strings

Project List Rendering
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

Master List Rendering
Master list rendering steps:
* clear list
* iterate over masterList
* render each row with:
  * rank
  * word
  * language
  * edit and delete controls
* append to Column D
Master list changes trigger:
* requestHighlightUpdate
* violations panel update

Cognate Window Rendering (Phase 2 Simple Cognate Window)
The cognate window has one section:
* detected cognates
Detected cognates rendering:
* clear detected section
* iterate over lemmas present in current text
* detect cognates using normalized lemmas
* render Spanish -> English pairs
* attach click handlers for highlight and master list insertion
Rules:
* cognate rendering must not trigger highlight directly
* cognate updates must call requestHighlightUpdate
Phase 2 constraints:
* no alphabetical dictionary
* no dictionary profiles
* no pending or official cognates
* no merged dictionary
* English lemma only is inserted

Violations Panel Rendering
Violations panel rendering steps:
* clear panel
* iterate over violations array
* render each violation with type and details
* append to panel
Violation types:
* unknown word
* out-of-order word
* curriculum gap

Event-Driven Rendering
Rendering is triggered by:
* requestHighlightUpdate
* click event in frequency list
* click event in cognate detected section
* edit event in master list
* save and load events
Rules:
* handleStableInput must never be called directly
* requestHighlightUpdate must debounce highlight
* no highlight during IME composition

Performance Considerations
Performance rules:
* avoid full re-renders
* avoid deep DOM trees
* keep token spans lightweight
* avoid nested spans
* avoid DOM mutation after render
The pipeline must remain fast enough for real-time typing.

Rendering and Analysis Integration
Rendering depends on analysis results. Analysis pipeline provides:
* tokens
* normalized lemmas
* cognate flags from simple detection
* frequency tiers
* curriculum ranks
* violation list
Rendering pipeline consumes these results and produces HTML.

Updated Architecture Rules
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

Adding New Rendering Features
When adding new rendering features:
* keep DOM structure simple
* avoid nested spans
* update highlight classes consistently
* update Detailed_Feature_Specifications.md
* update Developer_Workflow.md

Debugging Rendering Issues
Debugging steps:
* check tokenizer output
* check normalized lemmas
* check highlight classes
* check editor.innerHTML for nested spans
* check project list rendering
* check master list rendering
* check cognate window rendering
* check violations panel rendering

Summary
This rendering pipeline document explains how tokens become DOM nodes, how highlight classes are applied, how UI components re-render, and how rendering integrates with the analysis pipeline. It merges the original design with updated Phase 2 architecture rules, hybrid master list model, and July 19 fixes. All Phase 3 dictionary profile and alphabetical dictionary features have been removed to maintain stability.
