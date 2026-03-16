# Standards mapping — METADATA_TEMPLATE → Dublin Core / PROV-O / PREMIS / JSON-LD

This document maps the repository `METADATA_TEMPLATE.md` fields to recognised metadata/provenance standards (Dublin Core, PROV-O, PREMIS). It also shows a compact JSON-LD example and a PROV-O provenance snippet you can use as canonical examples for the project.

**Purpose:** students must show how the project's minimal metadata fields map to standard properties and how provenance is represented for RAG and archival workflows.

---

## Minimal fields (assumed from METADATA_TEMPLATE.md)

The repository expects these minimal metadata fields for every record:

- `record_id`
- `title`
- `author`
- `author_role`
- `creation_date`
- `purpose`
- `consent_status`
- `version_number`
- `change_history` (array of change entries)
- `accessibility_flags` (e.g., Easy-Read, Captioned, AAC)
- `evidence_references` (redacted in public examples)
- `watermark_indicator` (boolean / fingerprint)
- `export_provenance` (how/when exported: agent, date, sanitisation)

---

## Mapping table (field → Dublin Core / PROV-O / PREMIS / JSON-LD)

| Field | Dublin Core (dc/dcterms) | PROV-O | PREMIS | JSON-LD key & type | Notes / example value |
|---|---|---|---|---:|---|
| `record_id` | `dcterms:identifier` | `prov:entity (prov:id)` | `premis:objectIdentifier` | `"record_id": "CASE-2026-0001"` | Unique canonical ID for the record |
| `title` | `dc:title` | `prov:label` | `premis:objectCharacteristics` | `"title": "Advocacy Case — Housing Support"` | Human readable title |
| `author` | `dc:creator` | `prov:wasAttributedTo` | `premis:linkingAgentIdentifier` | `"author": {"name":"Alex River","orcid":""}` | Author identity; may be person or group |
| `author_role` | *custom* (use `dcterms:contributor` or `msa:authorRole`) | `prov:hadRole` | - | `"author_role":"Community Advocate"` | Controlled vocabulary recommended |
| `creation_date` | `dcterms:created` | `prov:generatedAtTime` | `premis:eventDateTime` | `"creation_date":"2026-03-01T00:00:00Z"` | ISO 8601 |
| `purpose` | `dcterms:description` | `prov:specializationOf` (or `prov:hadRole` metadata) | - | `"purpose":"Record of advocacy actions for housing assistance (fictional)"` | Short controlled-text field |
| `consent_status` | `dcterms:rights` (or custom `msa:consentStatus`) | `prov:wasInfluencedBy` (consent activity) | `premis:rights` | `"consent_status":"granted"` | Use controlled vocabulary `{granted, limited, withdrawn, unknown}` |
| `version_number` | `dcterms:hasVersion` | `prov:wasRevisionOf` (link to prior entity) | `premis:eventType` (versioning event) | `"version_number":"v1.0"` | Semantic versioning preferred |
| `change_history` | `dcterms:provenance` | `prov:Activity` chain (`prov:wasInfluencedBy`) | `premis:event` entries | `"change_history":[{"version":"v1.1","date":"2026-03-10","by":"J.Doe","summary":"Small edits"}]` | Change log with initials & ISO timestamps |
| `accessibility_flags` | `dcterms:audience` (or custom `msa:accessibility`) | `prov:hadRole` (accessibility -> output) | - | `"accessibility_flags":["Easy-Read","Captioned"]` | Use controlled vocabulary |
| `evidence_references` | `dcterms:references` | `prov:wasDerivedFrom` | `premis:objectCharacteristics` | `"evidence_references":["EVID-0001 (redacted)"]` | Always redacted in public examples |
| `watermark_indicator` | custom `msa:watermark` | `prov:hadRole` (as digital signature) | - | `"watermark_indicator":true` | True if author watermark or fingerprint present |
| `export_provenance` | `dcterms:provenance` | `prov:wasGeneratedBy` | `premis:event` | `"export_provenance":{"agent":"ExportTool v1.0","date":"2026-03-12","sanitised":true}` | Record how and when an export was created |

> **Note:** `msa:` is the project namespace (Metadata Sovereignty Alliance). For JSON-LD examples we define `msa` in the `@context`.

---

## Controlled vocabularies (recommended)

- `consent_status`: `["granted", "limited", "withdrawn", "unknown"]`  
- `author_role`: `["self-advocate","advocate","researcher","supervisor","clinician","community-member"]`  
- `accessibility_flags`: `["Easy-Read","Captioned","Auslan","AAC","Plain-Language"]`

Maintain a short list in `CONTROLLED_VOCABULARIES.md` (students should reference it).

---

## JSON-LD example (record + provenance)

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
