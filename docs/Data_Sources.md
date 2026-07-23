Data Sources — WordList Writer
Version: 2026-07-23
Status: Authoritative Source of Truth

Overview
This document defines all linguistic data sources used by WordList Writer. It reflects the simplified Phase 2 architecture: English-only analysis, simple cognate window, stable normalization rules, stable tokenizer rules, and Supabase-backed master list storage. All data sources must remain transparent, reproducible, and aligned with the analysis pipeline.

1. English Data Sources
NGSL — New General Service List
* public domain
* clean, modern, beginner-appropriate
* primary English frequency list
* stored as: frequency/english_ngsl.json

English Master List Source
* based on NGSL principles
* stored as: public/master/master_list.json
* English-only during Phase 2

English Lemma Map
* custom-generated or adapted from open-source datasets
* maps inflected → lemma
* stored as: lemmas/english.json

2. Spanish Data Sources (Phase 2 Simple Cognates Only)
Spanish Cognate List
* Spanish → English cognates
* public domain etymology sources
* stored as: cognates/english_spanish.json
Purpose:
* supports simple cognate detection
* used only for green underline highlighting
* no tier metadata
* no dictionary profiles
* no alphabetical dictionary

Spanish Frequency List (Deferred to Phase 3)
* stored as: frequency/spanish.json
Spanish Lemma Map (Deferred to Phase 3)
* stored as: lemmas/spanish.json

3. Koine Greek Data Sources (Deferred to Phase 3)
Greek Frequency List
* stored as: frequency/greek.json

Greek Lemma Map
* stored as: lemmas/greek.json

4. Latin Data Sources (Deferred to Phase 3)
Latin Frequency List
* stored as: frequency/latin.json

Latin Lemma Map
* stored as: lemmas/latin.json

5. Cognate Lists (Phase 2)
Cognate lists support simple cognate detection only.

English ↔ Spanish
* based on shared Latin roots
* high-frequency cognates
* stored as: cognates/english_spanish.json

Phase 2 Rules:
* no tier metadata
* no dictionary profiles
* no alphabetical dictionary
* no merged dictionary
* no pending/official cognates
* no publish workflow

6. Tokenization Rules (Phase 2)
English
* whitespace + punctuation splitting
* lowercase normalization
* basic Unicode normalization
Rules:
* tokenizer must not trigger highlight or autosave
* tokenizer must preserve spacing
* tokenizer must preserve punctuation
* tokenizer must return objects only for word tokens
* tokenizer must return raw strings for punctuation and whitespace

7. Normalization Rules (Phase 2)
All English lemmas:
* lowercase
* Unicode NFD
* remove diacritics
* remove punctuation for lemma lookup
Purpose:
* ensures consistent lookup across frequency lists, lemma maps, cognate maps, and master list

Normalization collisions:
* must be treated as duplicates
* must be logged during data ingestion

8. Master List Storage (Phase 2)
Primary file:
* public/master/master_list.json

Backup directory:
* public/master/backups/
* timestamped backups created on every save

Supabase Master List (Internal)
* stored in master_wordlists table
Fields:
* lemma
* rank
* language
* project_id
Rules:
* masterList must contain plain strings only
* Supabase rejects rows where lemma is undefined
* frontend assigns masterList directly from returned rows

9. Project Wordlist Storage (Phase 2)
Supabase Project Wordlist
* stored in project_wordlists table
Fields:
* lemma
* language
* project_id
Rules:
* normalized lemmas only
* no duplicates
* used for Column C display

10. Frequency Set Rules (Phase 2)
* NGSL-1K must contain exactly 1000 normalized lemmas
* rank continuity required
* normalization collisions must be detected
* frequencySet contains normalized lemmas only
Purpose:
* ensures known-word detection is accurate and consistent

11. Data Integrity Rules
All data sources must:
* use normalized lemmas
* avoid duplicates
* maintain rank continuity
* avoid malformed entries
* avoid null values
* avoid mixed casing

12. Summary
WordList Writer uses transparent, reproducible, public-domain or open-source linguistic data. All frequency lists, lemma maps, cognate lists, and master lists are clearly organized and easy to update. This Phase 2 data source specification defines the authoritative sources and normalization rules required for consistent English-only analysis.
