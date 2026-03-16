# Metadata Sovereignty System Architecture (Conceptual)

This diagram illustrates the conceptual architecture being explored in this project.

The system prioritises **metadata-first record infrastructure** with optional AI tools layered on top to support accessibility and summarisation.

AI tools should assist users while preserving:

• authorship  
• consent  
• traceability  
• accessibility  

---

## Conceptual Architecture

```mermaid
flowchart TD

A[User Creates Record] --> B[Metadata Capture]

B --> C[Record Repository]

C --> D[Version History]
C --> E[Consent Management]
C --> F[Authorship Tracking]

C --> G[AI Processing Layer]

G --> H[Plain Language Summary]
G --> I[Easy Read Conversion]
G --> J[Caption Generation]

H --> K[Human Review]
I --> K
J --> K

K --> L[Accessible Output]

L --> M[Community Use]
Architecture Explanation
1. Record Creation

Records originate from:

• advocates
• researchers
• community members
• lived-experience documentation

Each record must capture metadata at creation.

2. Metadata Capture

Essential metadata includes:

• author
• author role
• creation date
• purpose of record
• consent status
• version number

This metadata ensures records remain traceable.

3. Record Repository

Records are stored in a structured archive that preserves:

• version history
• authorship information
• consent records

4. AI Processing Layer

AI features may be applied to records to support accessibility.

Possible features include:

• plain-language summaries
• Easy-Read conversion
• caption generation

These features must operate within ethical governance rules.

5. Human Review

AI outputs must be reviewed by humans before use.

This ensures:

• accuracy
• respect for author intent
• ethical safeguards.

6. Accessible Outputs

Final outputs may include:

• Easy-Read documents
• summaries
• captioned media

The goal is to improve accessibility and comprehension.

Design Principle

The architecture follows a metadata-first design.

AI tools operate on top of well-governed records rather than replacing record creation or decision-making.
