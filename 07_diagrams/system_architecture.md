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
