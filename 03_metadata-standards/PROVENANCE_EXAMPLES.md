# PROVENANCE_EXAMPLES.md

This document contains practical provenance examples for the project, using PROV-O (Turtle) and a short JSON-LD provenance fragment.  
Use these examples as templates when creating provenance chains for records (create → update → export → review → consent). Keep provenance records machine-readable and auditable.

**Intended location:** `/03_metadata-standards/PROVENANCE_EXAMPLES.md`  
**Note:** These examples are sanitized and fictional. For real records, include accurate agent identifiers, timestamps, and activity summaries.

---

## 1. Key modelling principles (short)

- Model each *event* (create / update / export / review / consent) as a **prov:Activity**.  
- Model people/tools as **prov:Agent**.  
- Model records/documents as **prov:Entity**.  
- Link activities to entities with `prov:wasGeneratedBy`, `prov:used`, and `prov:wasRevisionOf` where appropriate.  
- Capture times (`prov:startedAtTime`, `prov:endedAtTime`), associated agents (`prov:wasAssociatedWith`), and short human-readable summaries (use an `msa:note` or `rdfs:label`).  
- For exports, include sanitisation status and link to the redaction log (private) when redactions occur.

---

## 2. Simple creation activity — Turtle (PROV-O)

```turtle
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix msa: <https://example.org/msa#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

# The record entity
<https://example.org/record/CASE-2026-0001> a prov:Entity ;
  prov:wasAttributedTo <https://example.org/agent/alex-river> ;
  prov:wasGeneratedBy <https://example.org/activity/create-CASE-2026-0001> ;
  msa:version "v1.0" .

# Creation activity
<https://example.org/activity/create-CASE-2026-0001> a prov:Activity ;
  prov:startedAtTime "2026-03-01T09:00:00Z"^^xsd:dateTime ;
  prov:endedAtTime "2026-03-01T09:05:00Z"^^xsd:dateTime ;
  prov:wasAssociatedWith <https://example.org/agent/alex-river> ;
  msa:note "Initial creation of fictional test record." .

# Agent (creator)
<https://example.org/agent/alex-river> a prov:Agent ;
  msa:name "Alex River" .
