# 📄 **PRD.md — WordList Writer (Node.js + Express + Supabase)**

## 1. Purpose  
WordList Writer is a multilingual writing tool that helps learners produce texts using **beginner‑level vocabulary** across English, Spanish, Koine Greek, and Latin. It provides:

- Real‑time vocabulary checking  
- Cognate awareness  
- Frequency‑based highlighting  
- Custom project word lists  
- Master vocabulary tracking across languages  

The goal is to help learners write texts using **high‑frequency, high‑value vocabulary**, while recognizing **cross‑language cognates** and building a personalized vocabulary profile.

---

## 2. Core Features

### 2.1 Writing Window (Bottom Panel)  
- Large text input area  
- Real‑time tokenization  
- Lemmatization based on selected language  
- Highlighting rules (strict priority):  
  - **Green** = cognate  
  - **Black/normal** = in project list or frequency list  
  - **Red** = off‑list  

### 2.2 Top Panel (Three Columns)

#### Column A — Frequency List  
- Displays active frequency list  
- Sorted by rank  
- Click → highlight in writing window  

#### Column B — Cognate Window  
- Displays cognates relevant to selected language  
- Supports: English ↔ Spanish/Latin/Greek  
- Click → highlight in writing window  

#### Column C — Project Word List  
- Custom list per project  
- User‑editable  
- Stored in Supabase  

Master List (Column D)
The Master List is a stable, curated vocabulary sequence used across all languages supported by the app. It defines the beginner‑level lemmas (initially ~400) and their equivalents in English, Spanish, Latin, and Greek.

Master List Fields
master_rank (1–400)

english_lemma

spanish_lemma

latin_root

greek_root

cognate_flags

frequency_ranks (per language)

Behavior
The Master List is displayed in Column D.

Highlighting in the writing window uses Master List membership and rank.

Red = not in Master List or used too early.

Black = allowed (in Master List or Project List).

Green = cognate (overrides other colors).

Hovering shows Master List rank or “Not in Master List.”

Editing
The user may modify the Master List directly inside Column D:

add new lemmas

insert lemmas at specific ranks

reorder lemmas

add cross‑language equivalents

update cognate flags

Changes update highlighting immediately.
---

## 3. Multilingual Capability

### Supported Languages  
- English  
- Spanish  
- Koine Greek  
- Latin  

### Language Modules (Static JSON)  
- `frequency/<language>.json`  
- `lemmas/<language>.json`  
- `cognates/<language>.json`  

### Tokenizer Rules  
- English/Spanish: whitespace + punctuation  
- Greek/Latin: Unicode‑aware, accent‑aware  

---

## 4. Highlighting Logic (Priority Order)

1. **Cognate (Green)**  
2. **Project Word List (Black)**  
3. **Frequency List (Black)**  
4. **Off‑List (Red)**  

---

## 5. Word Lists

### 5.1 Frequency Lists (Static JSON)

#### ⭐ English Frequency List Source (NGSL)  
The app uses the **public‑domain NGSL** from GitHub:

`https://github.com/neilblom/wordlist-writer/tree/main/ngsl`

Stored as:

```
frequency/english_ngsl.json
```

### 5.2 Lemma Maps  
Maps inflected forms → lemma.

### 5.3 Cognate Lists  
English ↔ Spanish/Latin/Greek.

### 5.4 Project Word Lists (Supabase)  
Stored per project.

### 5.5 Master Lists (Supabase)

#### A. Master List per Language  
Tracks all words used across all projects.

#### B. Cross‑Language Master List  
Stores:

- English lemma  
- Spanish equivalent  
- Latin root  
- Greek root  
- Cognate flags  
- Frequency ranks  

---

## 6. Architecture Overview

### Frontend  
- HTML/CSS/JS  
- Three top windows + bottom writing window  
- Supabase client for project storage  

### Backend — Node.js + Express  
- Serves static frontend  
- Serves static JSON lists  
- API endpoints for project/master list operations  

### Database — Supabase  
Tables:

- `projects`  
- `project_wordlists`  
- `master_wordlists`  
- `cross_language_master`  

---

## 7. Roadmap  
### Phase 5 — Deployment (Render)
