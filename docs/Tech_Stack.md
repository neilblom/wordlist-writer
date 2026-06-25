# 📄 **Tech_Stack.md — WordList Writer (Node.js + Express + Supabase)**

## Overview  
This document defines the complete technology stack for the WordList Writer rebuild. It ensures consistency, clarity, and long‑term maintainability across all development phases.

---

## **1. Frontend Stack**

### **HTML5**  
Used for the core structure of the UI, including:

- Three‑column top panel  
- Bottom writing window  
- Language selector  
- Word list displays  

### **CSS3**  
Handles layout and styling:

- Clean, minimal UI  
- Responsive three‑column layout  
- Highlighting colors (green, black, red)  
- Future dark‑mode support  

### **Vanilla JavaScript (ES6+)**  
Used for all client‑side logic:

- Real‑time tokenization  
- Lemma lookup  
- Highlighting logic  
- Loading JSON lists  
- Updating project word lists  
- Communicating with Supabase  

No frameworks (React/Vue/etc.) are used to keep the app lightweight and easy to deploy.

---

## **2. Backend Stack**

### **Node.js (LTS)**  
Primary runtime for the backend.

Reasons for choosing Node:

- Same language (JavaScript) on frontend and backend  
- Easy JSON handling  
- Fast development cycle  
- Excellent for lightweight APIs  
- Works perfectly with Render hosting  

### **Express.js**  
Backend framework used to:

- Serve static frontend files  
- Serve static JSON lists (frequency, lemmas, cognates)  
- Provide REST API endpoints for:
  - Loading/saving projects  
  - Updating master lists  
  - Fetching language modules  

### **File Structure (Backend)**  
```
src/
    server.js
    routes/
    controllers/
    utils/
public/
frequency/
lemmas/
cognates/
```

---

## **3. Database Stack**

### **Supabase (PostgreSQL)**  
Used for all persistent storage:

#### **Tables**
- `projects`  
- `project_wordlists`  
- `master_wordlists`  
- `cross_language_master`  

#### **Why Supabase?**
- Free tier is generous  
- Built‑in REST API  
- Easy JavaScript client  
- Real‑time updates  
- Secure row‑level policies  
- No server maintenance  

Supabase acts as the **source of truth** for:

- Project text  
- Project word lists  
- Master vocabulary tracking  
- Cross‑language relationships  

---

## **4. Static Data Files**

These are stored in the repo and served by Express:

### **Frequency Lists**
```
frequency/english_ngsl.json
frequency/spanish.json
frequency/greek.json
frequency/latin.json
```

### **Lemma Maps**
```
lemmas/english.json
lemmas/spanish.json
lemmas/greek.json
lemmas/latin.json
```

### **Cognate Lists**
```
cognates/english_spanish.json
cognates/english_latin.json
cognates/english_greek.json
```

Static JSON files are used because:

- They load instantly  
- They don’t require database queries  
- They rarely change  
- They keep the backend simple  

---

## **5. Hosting & Deployment**

### **Render (Backend Hosting)**  
Used to deploy the Node.js + Express server.

Reasons:

- Free tier available  
- Easy GitHub integration  
- Automatic redeploys  
- Environment variable support  
- Works well with Supabase  

### **Supabase (Database Hosting)**  
Handles all persistent data.

### **GitHub (Source Control)**  
Stores:

- Source code  
- Static JSON lists  
- Documentation (`/docs`)  
- Project history  

---

## **6. Development Tools**

### **VS Code**  
Recommended editor.

### **Git + GitHub**  
Version control and collaboration.

### **Node Version Manager (nvm)**  
Optional but recommended for managing Node versions.

---

## **7. Summary**

The WordList Writer tech stack is intentionally simple, modern, and maintainable:

- **Frontend:** HTML, CSS, Vanilla JS  
- **Backend:** Node.js + Express  
- **Database:** Supabase (PostgreSQL)  
- **Static Data:** JSON files  
- **Hosting:** Render + Supabase  
- **Source Control:** GitHub  

This stack ensures:

- Fast development  
- Easy debugging  
- Low hosting cost  
- Long‑term stability  
- No framework lock‑in
