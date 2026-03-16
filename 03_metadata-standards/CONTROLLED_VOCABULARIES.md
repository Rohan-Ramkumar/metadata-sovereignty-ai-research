# CONTROLLED_VOCABULARIES.md

This file defines the canonical controlled vocabularies used by the Metadata Sovereignty Alliance (msa) metadata schema.
Publish this file alongside `STANDARDS_MAPPING.md`. Use these vocabularies for validation, UI dropdowns, and JSON-LD URIs where programmatic validation is required.

**Namespace suggestion (example):** `https://example.org/msa/vocab#`  
(Replace with your production namespace if available.)

---

## How to use these vocabularies

- For **human-readable prototypes**, you may use strings (e.g., `"granted"`).  
- For **programmatic use / JSON-LD**, prefer term URIs, e.g.:  
  `"consent_status": "https://example.org/msa/vocab#granted"`  
  and include the same vocabulary URI in `@context` for compacting if needed.

When adding a new vocabulary term:
1. Add label, short definition and example usage to this file.
2. Add the term URI under your `msa` vocab namespace (or an approved public vocabulary).
3. Update `STANDARDS_MAPPING.md` referencing the vocabulary if required.

---

## 1. `consent_status` (msa:consentStatus)

**Purpose:** describes the current state of consent for usage of the record and permitted processing.

**Terms & definitions**
- `msa:granted` — Consent granted for stated uses. Use when the author explicitly consents to the metadata and stated processing.  
- `msa:limited` — Consent granted for limited/restricted uses only. Must document scope.  
- `msa:withdrawn` — Consent previously granted but later withdrawn. Data may require restriction or removal.  
- `msa:unknown` — Consent status unknown or not collected (use only when appropriate and flagged).

**Example (JSON-LD string form):**
```json
"msa:consentStatus": "msa:granted"
Human-readable example:
consent_status: "granted"
