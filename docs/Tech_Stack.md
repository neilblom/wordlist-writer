Data Sources — WordList Writer
Version: 2026-07-17
Status: Authoritative Source of Truth

1. English Data Sources
NGSL — New General Service List
* Public domain
* Clean, modern, beginner-appropriate
* Primary English frequency list
* Stored as: frequency/english_ngsl.json

English Master List Source
* Based on NGSL/NAWL/BNC/COCA principles
* Stored as: data/master_list.json
* English-only until multilingual expansion

English Lemma Map
* Custom-generated or adapted from open-source datasets
* Maps inflected → lemma
* Stored as: lemmas/english.json

2. Spanish Data Sources
Spanish Frequency List
* Recommended sources:
  * SUBTLEX-ESP
  * Wiktionary frequency dumps
  * OpenSubtitles Spanish lists
* Stored as: frequency/spanish.json

Spanish Lemma Map
* Recommended sources:
  * Freeling morphological analyzer
  * Wiktionary lemma mappings
  * Open-source Spanish NLP datasets
* Stored as: lemmas/spanish.json

3. Koine Greek Data Sources
Greek Frequency List
* Recommended sources:
  * Open Greek and Latin Project
  * MorphGNT (SBLGNT counts)
  * Perseus frequency data
* Stored as: frequency/greek.json

Greek Lemma Map
* Sources:
  * MorphGNT lemma mappings
  * Perseus morphological data
* Stored as: lemmas/greek.json

4. Latin Data Sources
Latin Frequency List
* Recommended sources:
  * Dickinson College Core Vocabulary (DCC)
  * Perseus Latin frequency data
  * Wiktionary Latin frequency lists
* Stored as: frequency/latin.json

Latin Lemma Map
* Sources:
  * Perseus morphological data
  * Whitaker’s Words (public domain)
* Stored as: lemmas/latin.json

5. Cognate Lists
English ↔ Spanish
* Based on shared Latin roots
* High-frequency cognates
* Public domain etymology sources
* Stored as: cognates/english_spanish.json

English ↔ Latin
* Based on Latin root dictionaries
* Public domain etymology data
* Stored as: cognates/english_latin.json

English ↔ Greek
* Based on Greek root dictionaries
* Public domain etymology data
* Stored as: cognates/english_greek.json

6. Tokenization Rules
English/Spanish
* Whitespace + punctuation splitting
* Lowercasing
* Basic normalization

Greek/Latin
* Unicode normalization
* Accent stripping
* Combining diacritic handling

7. Master List Storage
Primary file:
* public/master/master_list.json

Backup directory:
* public/master/backups/
* Timestamped backups created on every save

Supabase Master List (Internal)
* Stored in master_wordlists table
* Fields: lemma, rank, language, is_cognate, project_id
* Populated from frontend Master List objects
* Used for comparison logic and global list generation

8. Summary
WordList Writer uses transparent, reproducible, public-domain or open-source linguistic data. All frequency lists, lemma maps, cognate lists, and master lists are clearly organized and easy to update. As new lists are added or refined, this document must be updated to reflect current sources.

Documentation Formatting Reminder
All documentation updates must follow this formatting standard:
* Use plain text section titles
* Use asterisks (*) for bullet points
* Do not insert blank lines inside bullet lists
* Use ASCII-only characters
* Avoid Markdown headings (#)
* Avoid fenced code blocks unless necessary
* Use Step format for workflows to prevent GitHub auto-renumbering
This ensures consistent rendering across GitHub and prevents formatting breakage.
