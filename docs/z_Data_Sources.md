Data Sources — WordList Writer
Version: 2026-07-20
Status: Authoritative Source of Truth

Overview
This document defines all linguistic data sources used by WordList Writer. It merges the original data source list with updated normalization rules, tokenizer rules, dictionary profile system, hybrid cognate window, alphabetical dictionary, cognate merging rules, and July 19 fixes. All data sources must remain transparent, reproducible, and aligned with the analysis pipeline.

1. English Data Sources
NGSL — New General Service List
* public domain
* clean, modern, beginner-appropriate
* primary English frequency list
* stored as: frequency/english_ngsl.json
* future addition: BEL (Bible English List) from NGSL organization

English Master List Source
* based on NGSL, NAWL, BNC, COCA principles
* stored as: public/master/master_list.json
* English-only until multilingual expansion

English Lemma Map
* custom-generated or adapted from open-source datasets
* maps inflected → lemma
* stored as: lemmas/english.json

2. Spanish Data Sources
Spanish Frequency List
* recommended sources:
  * SUBTLEX-ESP
  * Wiktionary frequency dumps
  * OpenSubtitles Spanish lists
* stored as: frequency/spanish.json

Spanish Lemma Map
* recommended sources:
  * Freeling morphological analyzer
  * Wiktionary lemma mappings
  * open-source Spanish NLP datasets
* stored as: lemmas/spanish.json

3. Koine Greek Data Sources
Greek Frequency List
* recommended sources:
  * Open Greek and Latin Project
  * MorphGNT (SBLGNT counts)
  * Perseus frequency data
* stored as: frequency/greek.json

Greek Lemma Map
* sources:
  * MorphGNT lemma mappings
  * Perseus morphological data
* stored as: lemmas/greek.json

4. Latin Data Sources
Latin Frequency List
* recommended sources:
  * Dickinson College Core Vocabulary (DCC)
  * Perseus Latin frequency data
  * Wiktionary Latin frequency lists
* stored as: frequency/latin.json

Latin Lemma Map
* sources:
  * Perseus morphological data
  * Whitaker’s Words (public domain)
* stored as: lemmas/latin.json

5. Cognate Lists (Updated)
Cognate lists support dictionary profiles and hybrid cognate window.

English ↔ Spanish
* based on shared Latin roots
* high-frequency cognates
* public domain etymology sources
* stored as: cognates/english_spanish.json

English ↔ Latin
* based on Latin root dictionaries
* public domain etymology data
* stored as: cognates/english_latin.json

English ↔ Greek
* based on Greek root dictionaries
* public domain etymology data
* stored as: cognates/english_greek.json

Merged Dictionary (Runtime)
* built from official + pending cognates
* filtered by dictionaryProfile
* used for highlight pipeline
* used for hybrid cognate window
* used for alphabetical dictionary

6. Dictionary Profiles (Updated)
Dictionary profiles determine which cognates appear in Column B.

Profiles:
* spanish
* latin
* greek
* merged

Profile rules:
* stored per project
* affects cognate filtering only
* does not affect project language
* used by highlight pipeline and cognate window

7. Tokenization Rules (Updated)
English/Spanish
* whitespace + punctuation splitting
* lowercase normalization
* basic Unicode normalization
* tokenizer must not trigger highlight or autosave

Greek/Latin
* Unicode normalization (NFD)
* accent stripping
* combining diacritic handling
* tokenizer must preserve punctuation and whitespace

General Rules:
* tokenizer must preserve spacing
* tokenizer must preserve punctuation
* tokenizer must return objects only for word tokens
* tokenizer must return raw strings for punctuation and whitespace

8. Normalization Rules (Updated)
All languages
* lowercase
* Unicode NFD
* remove diacritics
* remove punctuation for lemma lookup
Purpose:
* ensures consistent lookup across frequency lists, lemma maps, cognate maps, dictionary profiles, and master list

Normalization collisions:
* must be treated as duplicates
* must be logged during data ingestion

9. Master List Storage (Updated)
Primary file:
* public/master/master_list.json

Backup directory:
* public/master/backups/
* timestamped backups created on every save

Supabase Master List (Internal)
* stored in master_wordlists table
* fields:
  * lemma
  * rank
  * language
  * is_cognate
  * project_id
Rules:
* masterList must contain plain strings only
* Supabase rejects rows where lemma is undefined
* frontend assigns masterList directly from returned rows

10. Cognate Storage (Updated)
Official Cognates:
* stored in cognates_official table
* used for highlight pipeline
* used for alphabetical dictionary

Pending Cognates:
* stored in cognates_pending table
* used for add/edit workflows
* merged into official on publish

Merged Dictionary:
* built at startup and after publish
* pending entries override official entries
* used for highlight pipeline
* used for hybrid cognate window
* used for alphabetical dictionary

11. Frequency Set Rules (Updated)
* NGSL-1K must contain exactly 1000 normalized lemmas
* NGSL-Full must contain exactly 2800 normalized lemmas
* rank continuity required
* normalization collisions must be detected
* frequencySet contains normalized lemmas only
Purpose:
* ensures known-word detection is accurate and consistent

12. Cross-Language Data (Updated)
Cross-language equivalents stored in:
* cross_language_master table

Fields:
* english
* spanish
* latin
* greek
* cognate_flags
* frequency_ranks

Purpose:
* supports multilingual curriculum design
* supports future multilingual master lists

13. Data Integrity Rules
All data sources must:
* use normalized lemmas
* avoid duplicates
* maintain rank continuity
* maintain alphabetical sorting where required
* avoid malformed entries
* avoid null values
* avoid mixed casing

14. Summary
WordList Writer uses transparent, reproducible, public-domain or open-source linguistic data. All frequency lists, lemma maps, cognate lists, dictionary profiles, merged dictionaries, and master lists are clearly organized and easy to update. This document defines the authoritative sources and normalization rules required for consistent analysis across languages.

Documentation Formatting Reminder
* use plain text section titles
* use asterisks (*) for bullet points
* do not insert blank lines inside bullet lists
* use ASCII-only characters
* avoid Markdown headings (#)
* avoid fenced code blocks unless necessary
* use Step format for workflows to prevent GitHub auto-renumbering
