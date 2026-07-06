# 📄 **Next_Steps.md — WordList Writer**

## Overview  
This document outlines the immediate next steps required to begin development on the WordList Writer rebuild. These steps follow directly from the PRD, Roadmap, and Tech Stack decisions.

---

# ⭐ **1. Set Up the Project Structure**

### Create the base folder layout:
```
/src
    server.js
    /routes
    /controllers
    /utils

/public
    index.html
    styles.css
    app.js

/frequency
/lemmas
/cognates

/docs
```

### Why this matters  
This structure keeps backend, frontend, and static data cleanly separated, making the project easy to navigate and maintain.

---

# ⭐ **2. Initialize Node.js + Express**

### Steps:
- Run `npm init -y`
- Install Express
- Create `src/server.js`
- Configure Express to:
  - Serve static files from `/public`
  - Serve JSON files from `/frequency`, `/lemmas`, `/cognates`
  - Provide a basic health‑check route

### Goal  
A running local server that displays `index.html`.

---

# ⭐ **3. Add the English Frequency List (NGSL)**

### Steps:
- Create `frequency/english_ngsl.json`
- Add the NGSL data
- Load it in the frontend using `fetch()`

### Goal  
The frequency list appears in Column A.

---

# ⭐ **4. Add the English Lemma Map**

### Steps:
- Create `lemmas/english.json`
- Add lemma mappings
- Load it in the frontend

### Goal  
Words in the writing window resolve to their lemmas.

---

# ⭐ **5. Implement Tokenizer + Highlighting Logic**

### Steps:
- Build tokenizer in `app.js`
- Build lemma lookup
- Implement highlighting rules:
  1. Green = cognate  
  2. Black = frequency or project list  
  3. Red = off‑list  

### Goal  
English‑only highlighting works end‑to‑end.

---

# ⭐ **6. Build the Three‑Column UI**

### Steps:
- Column A: Frequency list  
- Column B: Cognates (placeholder for now)  
- Column C: Project word list (placeholder for now)  

### Goal  
The UI layout matches the PRD.

---

# ⭐ **7. Prepare for Supabase Integration**

### Steps:
- Create Supabase project  
- Create tables:
  - `projects`
  - `project_wordlists`
  - `master_wordlists`
  - `cross_language_master`
- Add environment variables to Render (later)

### Goal  
Database ready for Phase 4.

---

# ⭐ **8. Begin Phase 1 Development**

Phase 1 = English‑only prototype.

### Deliverable:
A working version of WordList Writer with:

- Writing window  
- Frequency list  
- Lemma lookup  
- Highlighting  
- Basic UI  

This becomes the foundation for all future phases.

# ⭐ **9. 

### Multilingual Expansion (Future)
- Add Spanish cognates
- Add Latin cognates
- Add Greek cognates
- Extend tokenizer and lemmaMap to support additional languages
- Revisit master list structure after Supabase integration


# ⭐ Summary  
These steps move the project from documentation → implementation.  
Once Phase 1 is complete, the app will already be usable in English and ready for cognates, multilingual support, and Supabase integration.
