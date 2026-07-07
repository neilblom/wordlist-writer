# 📄 **Glossary.md — WordList Writer**

## Overview  
This glossary defines all key terms used throughout the WordList Writer project. It ensures clarity and consistency across documentation, development, and future updates.

---

# ⭐ **A**

### **Accent Stripping**  
The process of removing diacritical marks from characters (e.g., Greek or Latin) to normalize text for comparison and lemma lookup.

API Route Shadowing  
When static middleware intercepts requests before API routes, causing 404 and HTML fallback.

---

# ⭐ **C**

### **Cognate**  
A word in another language that shares a common historical root with an English word.  
Examples:  
- English *information* ↔ Spanish *información*  
- English *manual* ↔ Latin *manus*  
- English *biology* ↔ Greek *βίος* (bios)

Cognates receive **green highlighting** (highest priority).

COGNATE_MAP
Unified lookup map containing { es, tier } for each English lemma.
Built at startup from cognate JSON files + tier table.

---

### **Cognate Window**  
The middle column in the top panel that displays cognates relevant to the selected language. Clicking a cognate highlights it in the writing window.

---

### **Cross‑Language Master List**  
A Supabase table that stores relationships between English, Spanish, Latin, and Greek words, including:

- Lemmas  
- Cognate flags  
- Frequency ranks  
- Shared roots  

Used to track vocabulary across all languages.

---

# ⭐ **F**

### **Frequency List**  
A ranked list of the most common words in a language.  
Example: NGSL for English.

Used to determine whether a word is:

- Allowed (black)  
- Off‑list (red)  

Stored as static JSON files.

---

### **Frequency Rank**  
A number indicating how common a word is in a language.  
Lower rank = more common.

Example:  
- *the* → rank 1  
- *information* → rank ~800  
- *astronomy* → rank ~4000  

---

# ⭐ **H**

Hard Refresh  
Browser reload that bypasses cache (Ctrl+Shift+R), required when JS changes aren’t loading.

### **Highlighting Logic**  
The rules that determine how each word in the writing window is colored:

1. **Green** — Cognate  
2. **Black** — In project list or frequency list  
3. **Red** — Off‑list  

This priority order is strict.

---

# ⭐ **L**

### **Lemma**  
The base dictionary form of a word.

Examples:  
- running → run  
- went → go  
- children → child  
- mejores → mejor  

Lemmas are used to check frequency lists and cognate lists.

---

### **Lemma Map**  
A JSON file mapping inflected forms to their lemmas.

Example entry:
```
"running": "run"
```

Each language has its own lemma map.

---

# ⭐ **M**

### **Master List**  
A per‑language list stored in Supabase that tracks every lemma the user has ever used across all projects.

Used to build long‑term vocabulary profiles.

---

### Master List Item (English-only)
- `word`: the lemma
- `language`: always "english"
- `rank`: numeric position
- `cognate`: optional boolean
- `cognates`: optional object mapping language → lemma

# ⭐ **N**

Normalization
Process of converting text to NFD form and removing diacritics for consistent lookup.

# ⭐ **P**

### **Project**  
A saved writing session that includes:

- Project text  
- Project word list  
- Language  
- Timestamp  

Stored in Supabase.

---

### **Project Word List**  
A custom list of words the user wants to allow for a specific project.  
Displayed in Column C of the top panel.

Words in this list are highlighted **black**.

---

# ⭐ **T**

Tier
A linguistic category assigned to a cognate: Latin, Greek, Biblical, or General.
Used for color‑coded highlighting and metadata display.

### **Tokenizer**  
The component that splits text into individual tokens (words).  
Rules vary by language:

- English/Spanish: whitespace + punctuation  
- Greek/Latin: Unicode‑aware, accent‑aware  

---

### **Token**  
A single unit of text produced by the tokenizer.  
Example:  
“running,” → token = “running”

---

# ⭐ **W**

### **Writing Window**  
The large bottom panel where the user types text.  
Words are highlighted in real time based on:

- Cognates  
- Project list  
- Frequency list  

---

# ⭐ Summary  
This glossary defines the core terminology used throughout the WordList Writer project. It should be updated whenever new concepts, lists, or features are introduced.
