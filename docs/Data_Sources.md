# 📄 **Data_Sources.md — WordList Writer**

## Overview  
This document lists all linguistic data sources used in WordList Writer, including frequency lists, lemma maps, and cognate lists. It ensures transparency, reproducibility, and long‑term maintainability.

---

# ⭐ **1. English Data Sources**

## **1.1 NGSL — New General Service List (English Frequency List)**  
**Source:**  
`https://github.com/neilblom/wordlist-writer/tree/main/ngsl` [(github.com in Bing)](https://www.bing.com/search?q="https%3A%2F%2Fgithub.com%2Fneilblom%2Fwordlist-writer%2Ftree%2Fmain%2Fngsl")

**Notes:**  
- Public domain  
- Clean, modern, beginner‑appropriate  
- Used as the **primary English frequency list**  
- Stored as:  
  ```
  frequency/english_ngsl.json
  ```

---
### English Master List Source
- Based on NGSL/NAWL/BNC/COCA principles
- Stored as JSON in /data/master_list.json
- English-only for now; multilingual expansion planned




## **1.2 English Lemma Map**  
**Source:**  
Custom‑generated or adapted from open‑source morphological datasets.

**Purpose:**  
Maps inflected forms → lemma.

**Stored as:**  
```
lemmas/english.json
```

---

# ⭐ **2. Spanish Data Sources**

## **2.1 Spanish Frequency List**  
**Source:**  
To be selected — recommended options include:

- SUBTLEX‑ESP (open academic dataset)  
- Wiktionary frequency dumps  
- OpenSubtitles Spanish frequency lists  

**Stored as:**  
```
frequency/spanish.json
```

---

## **2.2 Spanish Lemma Map**  
**Source:**  
To be selected — recommended options:

- Freeling morphological analyzer  
- Wiktionary lemma mappings  
- Open‑source Spanish NLP datasets  

**Stored as:**  
```
lemmas/spanish.json
```

---

# ⭐ **3. Koine Greek Data Sources**

## **3.1 Greek Frequency List**  
**Source:**  
To be selected — recommended options:

- Open Greek and Latin Project  
- MorphGNT (SBLGNT frequency counts)  
- Perseus Project frequency data  

**Stored as:**  
```
frequency/greek.json
```

---

## **3.2 Greek Lemma Map**  
**Source:**  
- MorphGNT lemma mappings  
- Perseus morphological data  

**Stored as:**  
```
lemmas/greek.json
```

---

# ⭐ **4. Latin Data Sources**

## **4.1 Latin Frequency List**  
**Source:**  
To be selected — recommended options:

- Dickinson College Core Vocabulary (DCC)  
- Perseus Latin frequency data  
- Wiktionary Latin frequency lists  

**Stored as:**  
```
frequency/latin.json
```

---

## **4.2 Latin Lemma Map**  
**Source:**  
- Perseus Latin morphological data  
- Whitaker’s Words (public domain)  

**Stored as:**  
```
lemmas/latin.json
```

---

# ⭐ **5. Cognate Lists**

## **5.1 English ↔ Spanish Cognates**  
**Source:**  
Custom‑built list based on:

- Shared Latin roots  
- High‑frequency cognates  
- Public domain etymology sources  

**Stored as:**  
```
cognates/english_spanish.json
```

---

## **5.2 English ↔ Latin Cognates**  
**Source:**  
Custom‑built list using:

- Latin root dictionaries  
- Public domain etymology data  

**Stored as:**  
```
cognates/english_latin.json
```

---

## **5.3 English ↔ Greek Cognates**  
**Source:**  
Custom‑built list using:

- Greek root dictionaries  
- Public domain etymology data  

**Stored as:**  
```
cognates/english_greek.json
```

---

# ⭐ **6. Tokenization Rules**

## **6.1 English/Spanish**  
- Whitespace + punctuation splitting  
- Lowercasing  
- Basic normalization  

## **6.2 Greek/Latin**  
- Unicode normalization  
- Accent stripping  
- Combining diacritic handling  

---

# ⭐ **7. Master List Storage
- Primary file: public/master/master_list.json
- Backup directory: public/master/backups/
- Backup files created on every save
- Backup files contain full master list snapshot

Master List (Internal Data Source)
Stored in Supabase table master_wordlists.

Fields: lemma, rank, language, is_cognate, project_id.

Populated from frontend Master List objects.

Used for comparison logic and global list generation.

# ⭐ **8. Summary**

This document ensures that all linguistic data used in WordList Writer is:

- Transparent  
- Reproducible  
- Public‑domain or open‑source  
- Easy to update  
- Clearly organized  

As new lists are added or refined, this file should be updated to reflect the current sources.
