Tokenizer Specification
Version: 2026-07-20
Status: Authoritative Tokenizer Guide

Overview
This document defines the tokenizer architecture for WordList Writer. It explains how raw text becomes tokens, how normalization works, how IME-safe behavior is enforced, and how tokens integrate with the highlight pipeline, cognate system, frequency system, and master list. The tokenizer is a core subsystem and must remain stable across all future development phases.

1. Tokenizer Identity
The tokenizer is responsible for:
* splitting raw text into tokens
* preserving whitespace and punctuation
* producing normalized lemmas
* supporting IME-safe input
* providing token objects for analysis
The tokenizer must never trigger highlight or autosave.

2. Token Types
The tokenizer produces two token categories:
* word tokens (objects)
* non-word tokens (raw strings)
Word tokens contain:
* original: string
* normalized: string
* lemma: string
* type: "word"
Non-word tokens contain:
* raw punctuation
* raw whitespace
* type: "raw"

3. Tokenization Rules
Rules for splitting text:
* alphabetic sequences become word tokens
* punctuation becomes raw tokens
* whitespace becomes raw tokens
* numbers become word tokens
* hyphenated words remain intact
* apostrophes remain intact
Rules for preservation:
* preserve exact spacing
* preserve exact punctuation
* preserve original casing in output
Rules for safety:
* must not split alphabetic sequences incorrectly
* must not merge tokens
* must not remove whitespace

4. Normalization Rules
Normalization converts original text into lemmas. Rules:
* lowercase all letters
* remove trailing punctuation
* remove leading punctuation
* preserve apostrophes inside words
* preserve hyphens inside words
* convert accented characters to base forms
Normalization must:
* produce stable lemmas
* match frequencySet entries
* match cognate dictionary entries
* match master list entries

5. IME-Safe Behavior
IME rules:
* no tokenization during composition
* compositionstart sets isComposing = true
* compositionend triggers requestHighlightUpdate
* tokenizer runs only after IME commits text
IME safety prevents:
* prefix token logs
* partial tokenization
* corrupted highlight spans

6. Tokenizer Workflow
Step 1: read raw text from editor.innerText
Step 2: check IME state
Step 3: split text into raw tokens
Step 4: convert alphabetic sequences into word tokens
Step 5: normalize lemmas
Step 6: return token array to analysis pipeline

7. Integration With Highlight Pipeline
Tokenizer output feeds:
* cognate detection
* frequency detection
* master list detection
* violations detection
* highlight class computation
Rules:
* tokenizer must not call highlight
* tokenizer must not call autosave
* tokenizer must not mutate DOM
* tokenizer must not log normalized tokens

8. Integration With Cognate System
Cognate detection uses:
* normalized lemma
* dictionaryProfile
* merged dictionary
Rules:
* tokenizer must produce normalized lemmas that match dictionary keys
* tokenizer must not filter cognates
* tokenizer must not apply tiers
* tokenizer must not apply profile logic

9. Integration With Frequency System
Frequency detection uses:
* normalized lemma
* frequencySet
Rules:
* tokenizer must produce normalized lemmas that match frequencySet
* tokenizer must not compute frequency tiers
* tokenizer must not compute known-word status

10. Integration With Master List
Master list detection uses:
* normalized lemma
* masterSet
Rules:
* tokenizer must not modify master list
* tokenizer must not compute curriculum ranks
* tokenizer must not compute violations

11. Token Object Specification
Word token object:
* type: "word"
* original: string
* normalized: string
* lemma: string
* flags: optional analysis flags
Raw token:
* type: "raw"
* text: string

12. Error Handling
Tokenizer must:
* never throw exceptions during typing
* return empty array for empty text
* return raw tokens for malformed input
* avoid logging errors in production

13. Performance Rules
Performance requirements:
* tokenizer must run in under 5ms for typical text
* avoid regex backtracking
* avoid deep recursion
* avoid complex Unicode operations
* avoid unnecessary allocations

14. Updated Architecture Rules
Tokenizer invariants:
* tokenizeUnified must not trigger highlight or autosave
* tokenizeUnified must not log normalized tokens
* tokenizeUnified returns objects only for real word tokens
* punctuation and whitespace remain raw strings
* tokenizer must preserve spacing and punctuation
Highlight pipeline invariants:
* tokenizer output must be stable
* tokenizer must not mutate DOM
* tokenizer must not produce nested spans

15. Adding New Tokenizer Features
When extending tokenizer:
* update normalization rules consistently
* update Detailed_Feature_Specifications.md
* update Developer_Workflow.md
* ensure IME safety
* ensure highlight pipeline stability

16. Debugging Tokenizer Issues
Debugging steps:
* inspect raw text
* inspect token array
* inspect normalized lemmas
* check IME state
* check highlight classes
* check project list and master list behavior

17. Summary
This tokenizer specification defines how raw text becomes tokens, how normalization works, how IME-safe behavior is enforced, and how tokens integrate with the highlight pipeline, cognate system, frequency system, and master list. It is essential for maintaining stable analysis and rendering behavior across all future development phases.
