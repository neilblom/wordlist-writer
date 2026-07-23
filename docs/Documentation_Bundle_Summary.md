Documentation Bundle Summary — WordList Writer
Version: 2026-07-23
Status: Authoritative Overview

Purpose
This document provides a high-level summary of the entire WordList Writer documentation bundle. It explains how all subsystems fit together, how data flows through the application, and how each document contributes to the complete architecture. It is the single best entry point for understanding the whole system at once.

System Overview
WordList Writer is a teacher-facing authoring tool for creating controlled texts aligned with curriculum, frequency lists, and cognate scaffolding. The system consists of four major subsystems:

* frontend analysis and rendering
* backend persistence and API routing
* Supabase database schema
* curriculum and teacher workflows

All subsystems follow Phase 2 constraints:
* English-only master list
* simple cognate window
* no dictionary profiles
* no alphabetical dictionary
* no merged dictionary
* no pending/official cognates
* stable tokenizer and highlight pipeline
* single-layer editor

Core Pipelines
Tokenizer Pipeline
* splits text into tokens
* preserves whitespace and punctuation
* produces normalized lemmas
* IME-safe
* feeds highlight pipeline, project list, and master list detection

Highlight Pipeline
* centralized through requestHighlightUpdate
* applies cognate, known, and unknown classes
* updates violations panel
* triggers project list update
* rewrites editor.innerHTML

Rendering Pipeline
* converts tokens into HTML spans
* updates project list, master list, cognate window, and violations panel
* single-layer editor with no nested spans

Master List Workflow
* stores curriculum sequence
* English-only
* plain strings only
* insertion based on story order
* updates highlight pipeline and violations

Project List Workflow
* tracks lemmas used in current text
* updated after highlight
* used for teacher diagnostics

Supabase Integration
* projects table stores metadata
* project_wordlists stores lemmas used in text
* master_wordlists stores curriculum sequence
* saveEverything and loadEverything maintain project ID consistency

Teacher Workflow
* write text in editor
* observe highlight colors
* check violations panel
* add lemmas to master list
* use frequency list and cognate window for scaffolding
* save and load projects

Document Map
Backend_Architecture.md
* Express server structure
* API routes
* Supabase integration
* save/load pipelines

Frontend_Architecture.md
* single-layer editor
* highlight pipeline
* tokenizer rules
* rendering rules
* startup sequence

Tokenizer_Specification.md
* token types
* normalization rules
* IME safety
* integration with highlight pipeline

Rendering_Pipeline.md
* span generation
* highlight classes
* project list and violations rendering

Master_List_Workflow.md
* curriculum sequence logic
* insertion rules
* typed-word and cognate workflows

Teacher_Workflow.md
* teacher-facing usage
* highlight interpretation
* curriculum alignment

Detailed_Feature_Specifications.md
* internal behavior of all major systems
* highlight pipeline
* project list
* master list
* frequency ingestion

Decisions.md
* architectural decisions
* reasoning behind design choices

Roadmap.md
* development phases
* completed work
* future milestones

Glossary.md
* definitions of all project terminology

Setup_Supabase.md
* table creation
* environment variables
* connection tests
* save/load verification

Docs_Index.md
* full index of all documents
* purpose and usage of each file

Summary
This summary provides a complete high-level view of WordList Writer’s architecture. It explains how all documents relate to each other and how the system functions end-to-end. Use this file when restarting development, onboarding a developer, or reviewing the entire system at once.
