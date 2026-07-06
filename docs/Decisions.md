# 📄 **Decisions.md — WordList Writer**

## Overview  
This document records all major technical and architectural decisions made during the rebuild of WordList Writer. Each decision includes the reasoning behind it to ensure long‑term clarity and prevent repeated debates.

---

# ⭐ **1. Tech Stack Decisions**

### **Node.js + Express**
**Decision:** Use Node.js with Express for the backend.  
**Reasoning:**  
- Same language (JavaScript) on frontend and backend  
- Fast development  
- Easy JSON handling  
- Perfect for lightweight APIs  
- Works smoothly with Render hosting  

---

### **Vanilla JavaScript Frontend**
**Decision:** No React, Vue, or frameworks.  
**Reasoning:**  
- Faster load times  
- Lower complexity  
- Easier debugging  
- No build tools required  
- Perfect for a text‑processing app  

---

### **Supabase for Database**
**Decision:** Use Supabase (PostgreSQL) for all persistent storage.  
**Reasoning:**  
- Free tier  
- Built‑in REST API  
- Easy JavaScript client  
- Real‑time updates  
- Secure row‑level policies  
- No server maintenance  

---

# ⭐ **2. Data Architecture Decisions**

### **Static JSON Files for Frequency, Lemmas, Cognates**
**Decision:** Store linguistic data as static JSON files in the repo.  
**Reasoning:**  
- Fast to load  
- No database queries  
- Rarely changes  
- Easy to version control  
- Keeps backend simple  

---

Decision: API Routes Before Static Middleware  
Reason: Prevent static fallback from swallowing API requests.
Impact: Ensures /api/save-master-list is reachable.

Decision: Controlled Save Instead of Autosave  
Reason: Prevent accidental overwrites of curriculum.
Impact: User must explicitly confirm saves.

### **Supabase for Project + Master Lists**
**Decision:** Only user‑specific data goes into Supabase.  
**Reasoning:**  
- Keeps static data separate from user data  
- Ensures fast startup  
- Allows cross‑device syncing  
- Enables long‑term vocabulary tracking  

Decision: Master List Editing Occurs Inside the App UI (Option A)
Date: 2026‑06‑29
Status: Approved
Priority: High (Phase 3–4)

Summary
The Master List will be editable inside the app UI, not through external JSON editing. Column D will include controls that allow the user to:

add a new lemma to the Master List

insert a lemma at a specific rank

adjust ordering (move up/down)

add cross‑language equivalents

update cognate flags

update frequency metadata

All changes will immediately update:

the Master List display

the highlighting logic

the tooltip information

Supabase storage (Phase 4)

Rationale
The user’s workflow requires dynamic discovery: while writing texts in Spanish, Latin, or Greek, they may encounter lemmas that should be added to the beginner sequence. Manual JSON editing is too slow and error‑prone.

Editing inside the UI provides:

real‑time feedback

automatic highlighting updates

consistent multilingual behavior

scalable vocabulary development

a smooth workflow for building the cross‑language beginner list

Implications
Column D must support interactive editing.

Highlighting must update immediately when the Master List changes.

Supabase schema must include a master_list table with rank and multilingual fields.

Project List and Live Vocabulary remain separate concepts.

Master List becomes the authoritative vocabulary sequence across languages.

---

Decision: Master List is a Curriculum
The Master List is not alphabetical and not usage‑ordered.
It is a pedagogical sequence defined by the developer.

Decision: Cognate Insertion Rule
Cognates are inserted after the last Master List lemma that appears in the story, using lemma matching.

Decision: Typed Word Insertion Rule
New English lemmas typed in the story follow the same insertion rule as cognates.

Decision: Project List Behavior
The Project List tracks usage only.
It does not reorder or modify the Master List.

Decision: Warning System
The app warns when the story violates curriculum order.

Decision: Automatic Cognate Highlighting
Cognates are highlighted automatically with ✓.
No click‑to‑highlight feature.

---

## Master List Architecture Decision (July 2026)

**Current State:** English-only master list.

**Reasoning:**
- The highlight pipeline, tokenizer, lemmaMap, and order-checker are stable with English-only items.
- Multilingual fields (english/spanish/latin/greek) previously caused inconsistent item shapes and crashes.
- English-only structure is simple, predictable, and easy to maintain.

**Future Flexibility:**
- Multilingual support (Spanish, Latin, Greek) may be added later.
- Additional languages will be stored inside `cognates` object:
  {
    word: "and",
    language: "english",
    cognates: { spanish: "y", latin: "et", greek: "και" }
  }

**Commitment:**
- The master list will remain English-only until Phase 2 (Supabase integration) is complete.
- Revisit multilingual expansion after Phase 2.

Decision: Supabase Integration Before Multilingual Expansion
Date: 2026‑07‑06
Status: Approved
Priority: Critical (Foundation for Phase 3)

Summary  
The project will continue using the English‑only master list architecture while integrating Supabase. Multilingual support (Spanish, Latin, Greek) will be added after Supabase is fully implemented and stable.

This reverses the earlier assumption that multilingual support should precede database integration. The new direction reflects the actual dependency chain discovered during development.

Reasoning

The English‑only pipeline (tokenizer, lemmaMap, highlight logic, order‑checker) is stable and predictable.

Multilingual support requires persistent storage for:

master list ranks

cross‑language equivalents

cognate flags

frequency metadata

project word lists

These structures must live in Supabase to avoid duplication, corruption, and inconsistent state.

Implementing multilingual support before Supabase would require rewriting:

the master list

the cognate window

the highlight pipeline

the project list

the order‑checker

the backend

the UI

Supabase provides the stable foundation needed for Phase 3.

Implications

Phase 1 (English‑only pipeline) is considered complete.

Supabase integration becomes the next active development phase.

Multilingual support will be implemented after Supabase tables and API endpoints are stable.

The master list remains English‑only until Supabase is fully integrated.

All future sessions must follow this updated dependency order.

Updated Dependency Chain

English‑only pipeline (complete)

Supabase integration (current)

Multilingual support (next)

UI polish + deployment

# ⭐ **3. UI/UX Decisions**

### **Three‑Column Top Panel**
**Decision:** Frequency list (A), Cognates (B), Project list (C).  
**Reasoning:**  
- Mirrors the mental model of the user  
- Keeps all reference lists visible  
- Reduces clicks  
- Supports fast writing flow  

---

### **Bottom Writing Window**
**Decision:** Large, distraction‑free writing area.  
**Reasoning:**  
- Writing is the core activity  
- Needs maximum space  
- Real‑time highlighting requires clarity  

---

### **Highlighting Priority Rules**
**Decision:**  
1. Green = Cognate  
2. Black = In project list or frequency list  
3. Red = Off‑list  

**Reasoning:**  
- Cognates are pedagogically most valuable  
- Frequency + project lists are equally valid  
- Off‑list words must stand out

Decision: Add Column D (Master List) to the UI
Date: 2026‑06‑29
Status: Approved
Priority: High (Phase 2–3)

Summary
We will add a fourth column to the top panel of the app UI:

Column A: Frequency List

Column B: Cognates

Column C: Project List (historical, first‑appearance order)

Column D: Master List (stable, cross‑language)

Column D displays the curated cross‑language beginner vocabulary list (initially ~400 lemmas). It serves as the global target sequence for writing texts in English, Spanish, Latin, and Greek.

Rationale
The Master List is central to the long‑term goal of producing beginner‑level texts across multiple languages using a shared vocabulary sequence. The user needs a dedicated UI area to:

view the Master List

compare project vocabulary to the Master List

understand allowed vs. disallowed lemmas

see cross‑language equivalents

modify the Master List when necessary

The writing window already displays Live Vocabulary via highlighting, so no additional column is needed for that.

Implications
Column D becomes the authoritative source for beginner vocabulary.

Highlighting in the writing window must reflect Master List membership.

Red words indicate “not in Master List” or “too early.”

Column D will eventually support editing controls (see next decision).

Supabase will store the Master List in Phase 4.

---
Decision: Cognate Click Behavior
Chosen Approach:  
Clicking a cognate in Column B performs two actions:

Highlights all matching tokens in the writing window (green overlay).

Inserts the cognate into the Master List with correct language assignment.

Rationale:  
This creates a smooth workflow from discovery → selection → curation. It also reinforces the pedagogical goal of building a cross‑language vocabulary list.

Alternatives Considered:

Tooltip‑only behavior (rejected: too passive)

Separate “Add” button (rejected: slower workflow)

Decision: Project List Cognate Priority
Chosen Approach:  
Cognates appear at the top of Column C, marked with a green ✓ badge.

Rationale:  
Matches highlight priority rules (cognate > known > unknown).
Supports Phase 3 multilingual expansion.

Decision: Auto‑Scroll for Master List
Status: Deferred
Reason:  
Auto‑scroll depends on stable multilingual Master List behavior. Implementing now risks rework once Spanish/Latin/Greek frequency lists are added.

Action:  
Add to Roadmap Phase 2.5 (UI Polish). Implement after Phase 3 Step 2.
---
Decision: Spanish as First Multilingual Target
Chosen Approach:  
Spanish is implemented first because it has clean frequency data, reliable lemma resources, and strong cognate overlap with English. It also provides a modern language test case before adding classical languages.

Rationale:  
Spanish validates the multilingual architecture with minimal friction. Once Spanish works, Latin and Greek can be added with the same pipeline.

Decision: Frequency List Format
Chosen Approach:  
All frequency lists (English, Spanish, Latin, Greek) use the same structure:

json
{
  "rank": 1,
  "lemma": "example"
}
Rationale:  
Uniform structure simplifies loading, sorting, and highlight logic. It also ensures Master List frequency fields remain consistent across languages.

Decision: Lemma Map Format
Chosen Approach:  
All lemma maps follow the English Phase 1 structure:

json
{
  "hablo": "hablar",
  "hablas": "hablar",
  "habló": "hablar"
}
Rationale:  
Flat inflected → lemma mapping is fast, predictable, and works across all languages. No POS tags, no metadata, no nested objects.

Decision: Highlight Logic Extension
Chosen Approach:  
Highlight logic becomes language‑aware but preserves Phase 1/2 priority:

Cognate → green

Known (frequency list or project list) → black

Unknown → red

Rationale:  
This keeps the cognitive model consistent across languages.

Decision: Auto‑Scroll for Master List
Status: Deferred
Reason:  
Auto‑scroll depends on stable multilingual Master List behavior. Implementing now risks rework once Spanish/Latin/Greek frequency lists are added.

Action:  
Added to Roadmap Phase 2.5 (UI Polish). Implement after Phase 3.2.
---

# ⭐ **4. Multilingual Decisions**

### **Four Supported Languages**
**Decision:** English, Spanish, Koine Greek, Latin.  
**Reasoning:**  
- Covers modern + classical languages  
- Strong cognate relationships  
- Matches user’s long‑term goals  
- Balanced difficulty  

---

### **Language Modules Loaded Dynamically**
**Decision:** Load frequency, lemma, and cognate files based on selected language.  
**Reasoning:**  
- Reduces memory usage  
- Faster startup  
- Cleaner architecture  

---

# ⭐ **5. Deployment Decisions**

### **Render for Backend Hosting**
**Decision:** Deploy Node.js server on Render.  
**Reasoning:**  
- Free tier  
- GitHub auto‑deploy  
- Easy environment variable management  

---

### **Supabase for Database Hosting**
**Decision:** Use Supabase for all persistent data.  
**Reasoning:**  
- Zero maintenance  
- Built‑in auth (optional)  
- Real‑time features  
- PostgreSQL reliability  

---

# ⭐ **6. Repository Structure Decisions**

### **Use a /docs Folder**
**Decision:** Store all documentation in `/docs`.  
**Reasoning:**  
- Keeps repo organized  
- Provides a permanent memory  
- Matches industry standards

# ⭐ **7.Future Considerations: 
Mobile Support:

Decision: Mobile support is identified as a future expansion area but is not part of Phase 1–5.
Reasoning:

The current UI is optimized for desktop writing, with three fixed panels and a large writing window.

Mobile introduces constraints (screen size, touch input, limited simultaneous visibility) that require a dedicated design pass.

The core engine (tokenization, lemma lookup, highlighting, frequency logic) will already work on mobile once the UI is adapted.

A mobile‑friendly version may require:

Responsive layout

Collapsible panels

Touch‑optimized interactions

Local caching for offline use

Optional PWA wrapper

Status:  
Deferred until after Phase 5.
Will be revisited once the desktop version is stable and multilingual support is complete.

## Language Visibility Toggles (Future)

We will add UI controls that allow the user to toggle visibility of individual
language columns in the Master List (English, Spanish, Latin, Koine Greek, Modern
Greek, French, German, etc.). This prevents UI clutter and supports focused
multilingual workflows.

Status: Deferred until core CRUD features are stable.

Other future options:

🌟 Cloud sync across devices<br>🌟 Export to Kindle‑ready text<br>🌟 Image integration for illustrated stories<br>🌟 Simple animation script export<br>🌟 Collaboration mode (share project)<br>🌟 Advanced stats (reading level, sentence length, repetition heatmap)

---

### **Clean Rebuild in New Repo**
**Decision:** Delete old repo and start fresh.  
**Reasoning:**  
- Avoid legacy clutter  
- Avoid Ruby artifacts  
- Clean architecture from day one  

---

# ⭐ Summary  
This file captures all major decisions made during the rebuild.  
It should be updated whenever new architectural or design choices are finalized.
