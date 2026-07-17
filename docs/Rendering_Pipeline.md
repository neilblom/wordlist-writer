Rendering Pipeline
Version: 2026-07-17
Status: Authoritative Rendering Guide

Overview
This document explains how the rendering pipeline works in WordList Writer. It describes how tokens become DOM nodes, how highlight classes are applied, how overlays update, and how UI components re-render. It is designed for future development sessions and debugging UI update issues.

1. Rendering Philosophy
The rendering pipeline is designed to be:
* simple
* predictable
* incremental
* fast

The app avoids full-page re-renders. Only affected regions update after each keystroke.

2. Core Rendering Components
The rendering pipeline updates:
* writing window overlay
* project word list
* master list
* frequency list (rarely)
* cognate list (rarely)
* violations panel

Most updates occur in the writing window and project list.

3. Rendering Sequence
Rendering follows this order:
* tokenize text
* normalize lemmas
* run analysis pipeline
* compute highlight classes
* build overlay DOM
* update project list
* update violations panel
* update master list if needed

This order must remain stable.

4. Writing Window Rendering
The writing window uses two layers:
* raw text layer (textarea or contenteditable)
* overlay layer (highlighted tokens)

Overlay rendering steps:
* clear overlay
* iterate over tokens
* create span for each token
* apply highlight class
* append to overlay container

Highlight classes:
* highlight-cognate
* highlight-known
* highlight-unknown

5. Token Rendering Rules
Each token becomes a span element.

Rules:
* preserve original text
* apply class based on analysis
* maintain whitespace between tokens
* attach tooltip metadata

Tooltip metadata includes:
* lemma
* cognate tier
* frequency rank
* curriculum status

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
* highlight pipeline re-run
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
* input event in writing window
* click event in frequency list
* click event in cognate list
* edit event in master list
* save and load events

Each event triggers only the necessary re-renders.

10. Performance Considerations
Performance rules:
* avoid full re-renders
* avoid deep DOM trees
* reuse DOM nodes when possible
* keep overlay simple
* keep token spans lightweight

The pipeline must remain fast enough for real-time typing.

11. Rendering and Analysis Integration
Rendering depends on analysis results.

Analysis pipeline provides:
* tokens
* normalized lemmas
* cognate flags
* frequency tiers
* curriculum ranks
* violation list

Rendering pipeline consumes these results.

12. Adding New Rendering Features
When adding new rendering features:
* keep DOM structure simple
* avoid nested spans
* update highlight classes consistently
* update tooltip metadata
* update Detailed_Feature_Specifications.md
* update Developer_Workflow.md

13. Debugging Rendering Issues
Debugging steps:
* check tokenizer output
* check normalized lemmas
* check highlight classes
* check overlay DOM
* check project list rendering
* check master list rendering
* check violations panel rendering

14. Summary
This rendering pipeline document explains how tokens become DOM nodes, how highlight classes are applied, how overlays update, and how UI components re-render. It is essential for debugging UI update issues and safely extending the rendering system.

Documentation Formatting Reminder
* Use plain text section titles
* Use asterisks (*) for bullet points
* No blank lines inside bullet lists
* ASCII-only characters
* Avoid Markdown headings
* Avoid fenced code blocks unless necessary
* Use Step format for workflows
