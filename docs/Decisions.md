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

### **Supabase for Project + Master Lists**
**Decision:** Only user‑specific data goes into Supabase.  
**Reasoning:**  
- Keeps static data separate from user data  
- Ensures fast startup  
- Allows cross‑device syncing  
- Enables long‑term vocabulary tracking  

---

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

# ⭐ **7.Future Consideration: Mobile Support

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
