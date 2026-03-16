# STANDARDS_MAPPING.md

# Standards mapping — METADATA_TEMPLATE → Dublin Core / PROV-O / PREMIS / JSON-LD

This document defines how the repository `METADATA_TEMPLATE.md` fields map to established metadata and provenance standards (Dublin Core / DCTERMS, PROV-O, PREMIS). It also provides practical JSON-LD and PROV-O examples, implementation notes, and a short checklist students should satisfy before submitting schemas or example records.

**Intended location:** `/03_metadata-standards/STANDARDS_MAPPING.md`  
**Companion files recommended:** `/03_metadata-standards/CONTROLLED_VOCABULARIES.md` and `/example_records/ssa_case_jsonld_example.jsonld`.

---

## 1. Minimal metadata fields (canonical)

Every record should include the following minimal metadata fields:

- `record_id`  
- `title`  
- `author`  
- `author_role`  
- `creation_date`  
- `purpose`  
- `consent_status`  
- `version_number`  
- `change_history` (array of change entries)  
- `accessibility_flags` (e.g., `Easy-Read`, `Captioned`, `AAC`)  
- `evidence_references` (redacted in public examples)  
- `watermark_indicator` (boolean / fingerprint)  
- `export_provenance` (agent, date, sanitisation status)

---

## 2. Mapping table

This table shows a practical mapping from project fields to standard properties.

| Project field | Dublin Core / dcterms | PROV-O | PREMIS | JSON-LD key & type | Notes / example |
|---|---|---|---|---:|---|
| `record_id` | `dcterms:identifier` | `prov:entity (prov:id)` | `premis:objectIdentifier` | `"record_id": "CASE-2026-0001"` | Unique canonical identifier |
| `title` | `dc:title` | `prov:label` | `premis:objectCharacteristics` | `"title": "Advocacy Case — Housing Support"` | Human-readable title |
| `author` | `dc:creator` | `prov:wasAttributedTo` | `premis:linkingAgentIdentifier` | `"author": {"name":"Alex River","orcid":""}` | Person/group; respect anonymisation rules |
| `author_role` | `dcterms:contributor` or custom `msa:authorRole` | `prov:hadRole` | — | `"author_role":"Community Advocate"` | Controlled vocabulary recommended |
| `creation_date` | `dcterms:created` | `prov:generatedAtTime` | `premis:eventDateTime` | `"creation_date":"2026-03-01T00:00:00Z"` | ISO-8601 timestamp |
| `purpose` | `dcterms:description` | descriptive property or specialization link | — | `"purpose":"Record of advocacy actions for housing assistance"` | Short descriptive text |
| `consent_status` | `dcterms:rights` or `msa:consentStatus` | model as a consent activity (prov:Activity) | `premis:rights` | `"consent_status":"granted"` | Controlled vocabulary: `{granted, limited, withdrawn, unknown}` |
| `version_number` | `dcterms:hasVersion` | `prov:wasRevisionOf` | `premis:eventType` | `"version_number":"v1.0"` | Semantic versioning preferred |
| `change_history` | `dcterms:provenance` | chain of `prov:Activity` | `premis:event` | `"change_history":[{"version":"v1.1","date":"2026-03-10","by":"J.Doe","note":"Minor edits"}]` | Each entry: who, when, summary |
| `accessibility_flags` | `dcterms:audience` or `msa:accessibility` | attribute or role | — | `"accessibility_flags":["Easy-Read","Captioned"]` | Controlled vocabulary |
| `evidence_references` | `dcterms:references` | `prov:wasDerivedFrom` | `premis:objectCharacteristics` | `"evidence_references":["EVID-0001 (redacted)"]` | Must be redacted for public examples |
| `watermark_indicator` | custom `msa:watermark` | model as digital signature activity | — | `"watermark_indicator": true` | True if author watermark/fingerprint present |
| `export_provenance` | `dcterms:provenance` | `prov:wasGeneratedBy` | `premis:event` | `"export_provenance":{"agent":"ExportTool v1.0","date":"2026-03-12","sanitised":true}` | How and when export occurred |

> **Namespace note:** `msa:` is the recommended project namespace (Metadata Sovereignty Alliance). When serialising JSON-LD, include `msa` in `@context`.

---

## 3. Controlled vocabularies (short list)

Define and publish controlled vocabularies in `CONTROLLED_VOCABULARIES.md`. Example sets:

- `consent_status`: `["granted", "limited", "withdrawn", "unknown"]`  
- `author_role`: `["self-advocate","advocate","researcher","supervisor","clinician","community-member"]`  
- `accessibility_flags`: `["Easy-Read","Captioned","Auslan","AAC","Plain-Language"]`

**Recommendation:** Prefer short URIs for vocab terms in JSON-LD, or use strings for prototype simplicity.

---

## 4. JSON-LD example (sanitised record + provenance)

Use JSON-LD for web-friendly serialisation and RAG ingestion. This minimal example demonstrates the canonical fields and a provenance activity.

```json
{
  "@context": {
    "dc": "http://purl.org/dc/elements/1.1/",
    "dcterms": "http://purl.org/dc/terms/",
    "prov": "http://www.w3.org/ns/prov#",
    "premis": "http://www.loc.gov/premis/rdf/v1#",
    "msa": "https://example.org/msa#",
    "xsd": "http://www.w3.org/2001/XMLSchema#"
  },
  "@id": "https://example.org/record/CASE-2026-0001",
  "@type": "prov:Entity",
  "dcterms:identifier": "CASE-2026-0001",
  "dc:title": "Advocacy Case — Housing Support (Fictional)",
  "dc:creator": {
    "@type": "prov:Agent",
    "msa:name": "Alex River",
    "msa:role": "Community Advocate"
  },
  "dcterms:created": {
    "@value": "2026-03-01T00:00:00Z",
    "@type": "xsd:dateTime"
  },
  "msa:consentStatus": "fictional_test",
  "msa:version": "v1.0",
  "msa:accessibilityFlags": ["Easy-Read","Plain-Language"],
  "msa:changeHistory": [
    {
      "msa:version": "v1.0",
      "msa:date": "2026-03-01",
      "msa:by": "Alex River",
      "msa:note": "Created fictional example"
    }
  ],
  "prov:wasGeneratedBy": {
    "@id": "https://example.org/activity/create-CASE-2026-0001",
    "@type": "prov:Activity",
    "prov:startedAtTime": "2026-03-01T10:00:00Z",
    "prov:endedAtTime": "2026-03-01T10:05:00Z",
    "prov:wasAssociatedWith": {
      "@type": "prov:Agent",
      "msa:name": "Alex River"
    }
  }
}
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix msa: <https://example.org/msa#> .

<https://example.org/record/CASE-2026-0001> a prov:Entity ;
  msa:version "v1.1" ;
  prov:wasGeneratedBy <https://example.org/activity/create-CASE-2026-0001> ,
                    <https://example.org/activity/update-CASE-2026-0001> ;
  prov:wasAttributedTo <https://example.org/agent/alex-river> .

<https://example.org/activity/create-CASE-2026-0001> a prov:Activity ;
  prov:startedAtTime "2026-03-01T10:00:00Z" ;
  prov:endedAtTime "2026-03-01T10:05:00Z" ;
  prov:wasAssociatedWith <https://example.org/agent/alex-river> ;
  prov:used <https://example.org/record/template-ssa-casefile> .

<https://example.org/activity/update-CASE-2026-0001> a prov:Activity ;
  prov:startedAtTime "2026-03-10T09:00:00Z" ;
  prov:endedAtTime "2026-03-10T09:10:00Z" ;
  prov:wasAssociatedWith <https://example.org/agent/jane-smith> ;
  prov:used <https://example.org/record/CASE-2026-0001> ;
  prov:qualifiedActivity <https://example.org/activity/change-note-0001> .

<https://example.org/agent/alex-river> a prov:Agent ;
  msa:name "Alex River" .
