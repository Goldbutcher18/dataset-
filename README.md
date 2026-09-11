# Government Procedure Knowledge Base – Dataset 4

## Overview

Dataset 4 (D4) is a structured Government Procedure Knowledge Base designed for use in applications that need reliable information about Maharashtra government land, property, registration and related administrative procedures.

The dataset is intended for use in:

* Government procedure assistants
* Retrieval-Augmented Generation (RAG) systems
* Land-record information systems
* Property and registration information platforms
* Government-service chatbots
* AI-based document and procedure retrieval systems

The dataset currently contains **50 structured source records**.

---

## Key Coverage Areas

D4 includes government-source information related to:

* Land Acquisition
* Notifications
* Awards
* Compensation
* Possession
* Rehabilitation and Resettlement
* Objections and Hearings
* Legal Disputes
* Appeals and Revisions
* Survey and Demarcation
* Mutation / Ferfar
* 7/12 Extract
* 8A Extract
* Property Card
* CTS and ULPIN
* Land Records
* Property Registration
* Stamp Duty
* Property Valuation
* Agricultural Land
* Tenancy
* Land Use and Planning

---

## Dataset Structure

```text
project/
│
├── datasets/
│   └── D4_government_procedure_kb.json
│
├── scripts/
│   └── validate_d4.py
│
├── docs/
│   └── D4_DATASET_README.md
│
├── README.md
├── requirements.txt
└── .gitignore
```

---

## Dataset Schema

Each record follows the structure below:

```json
{
  "source_id": "MAH-MUTATION-PROCEDURE-001",

  "title": "Mutation Application and Certification Procedure",

  "department": "Revenue and Forest Department, Government of Maharashtra",

  "state": "Maharashtra",

  "district_scope": "State-wide",

  "document_type": "Official Procedure",

  "source_url": "https://example.gov.in",

  "publication_date": null,

  "effective_date": null,

  "language": "Marathi / English",

  "topic": "Mutation",

  "procedure_type": "Mutation Application",

  "document_categories": [
    "Mutation",
    "Ferfar",
    "Land Records"
  ],

  "content": "Description of the verified government source and procedure.",

  "verified_status": "search_confirmed",

  "source_authority": "Government of Maharashtra",

  "retrieved_at": "2026-09-11",

  "data_origin": "verified_public_source"
}
```

---

## Required Fields

The following fields are mandatory for every record:

| Field                 | Description                       |
| --------------------- | --------------------------------- |
| `source_id`           | Unique identifier for the source  |
| `title`               | Official or descriptive title     |
| `department`          | Responsible government department |
| `state`               | Geographic state                  |
| `district_scope`      | Geographic applicability          |
| `document_type`       | Type of government source         |
| `source_url`          | Official source URL               |
| `language`            | Language of the source            |
| `topic`               | Main knowledge topic              |
| `procedure_type`      | Specific procedure or workflow    |
| `document_categories` | Related categories                |
| `content`             | Source-grounded description       |
| `verified_status`     | Verification status               |
| `source_authority`    | Government authority              |
| `retrieved_at`        | Date the source was retrieved     |
| `data_origin`         | Source origin classification      |

Optional fields:

```text
publication_date
effective_date
```

---

## Source Verification Policy

D4 follows strict source-grounding rules.

### Rule 1 — Do Not Synthesize Legal Rules

The dataset must not invent, infer or synthesize legal rules.

If a procedure cannot be verified from an authoritative source:

```text
DO NOT ADD IT
```

---

### Rule 2 — Required Source Metadata

Every source must contain:

```text
source_url
source_authority
verified_status
retrieved_at
```

---

### Rule 3 — No Duplicate Knowledge

The same government document must not be duplicated simply to increase the dataset size.

Duplicate copies, mirrors or alternate downloads of the same document should not be treated as independent knowledge.

---

## Verification Status

Current records use:

```text
search_confirmed
```

This means the source was located and checked against an official government or government-authority publication during the dataset research process.

For future versions, the project may use statuses such as:

```text
search_confirmed
document_confirmed
official_pdf_confirmed
deprecated
needs_revalidation
```

---

## Example Usage in Python

```python
import json

with open(
    "datasets/D4_government_procedure_kb.json",
    "r",
    encoding="utf-8"
) as file:
    d4_dataset = json.load(file)

print("Total records:", len(d4_dataset))

for record in d4_dataset[:5]:
    print(record["title"])
    print(record["source_url"])
    print()
```

Expected output:

```text
Total records: 50
```

---

## Searching the Dataset

A simple keyword search can be implemented as follows:

```python
def search_dataset(query, dataset):
    query = query.lower()

    results = []

    for record in dataset:

        searchable_text = " ".join([
            record.get("title", ""),
            record.get("topic", ""),
            record.get("procedure_type", ""),
            record.get("content", ""),
            " ".join(record.get("document_categories", []))
        ]).lower()

        if query in searchable_text:
            results.append(record)

    return results
```

Example:

```python
results = search_dataset("mutation", d4_dataset)

for result in results:
    print(result["title"])
    print(result["source_url"])
```

---

## Recommended AI / RAG Workflow

For an AI assistant, the recommended workflow is:

```text
User Question
      │
      ▼
Query Processing
      │
      ▼
Search D4 Knowledge Base
      │
      ▼
Retrieve Relevant Sources
      │
      ▼
Extract Source-Grounded Content
      │
      ▼
Generate Response
      │
      ▼
Return Official Source URL
```

The application should return the relevant official source along with the generated answer whenever possible.

Example response structure:

```json
{
  "answer": "Based on the retrieved government procedure source...",
  "sources": [
    {
      "title": "Official Source Title",
      "authority": "Government of Maharashtra",
      "url": "https://official-source.gov.in"
    }
  ]
}
```

---

## Important Legal and Safety Notice

D4 is an information and retrieval dataset.

It is **not legal advice**.

The dataset should not be used to:

* Make final legal determinations
* Replace government authorities
* Replace advocates or legal professionals
* Automatically decide compensation or land ownership
* Generate unsupported legal rules

For high-stakes or legal decisions, users should be directed to the original official government source.

---

## Dataset Maintenance

Government procedures, notifications and service portals may change.

Recommended maintenance schedule:

```text
Source URL validation: Every 3 months

Government portal review: Every 6 months

Legal and notification review: Whenever a relevant update is published

Full dataset audit: Every 12 months
```

---

## Adding a New Record

Before adding a record:

1. Verify that the source is authoritative.
2. Check that the knowledge does not duplicate an existing source.
3. Confirm the procedure from the original government source.
4. Add all mandatory metadata.
5. Run the dataset validator.
6. Review the source URL manually.

Example:

```python
{
  "source_id": "MAH-NEW-PROCEDURE-001",
  "title": "Official Procedure Title",
  "department": "Government Department",
  "state": "Maharashtra",
  "district_scope": "State-wide",
  "document_type": "Official Procedure",
  "source_url": "https://official-government-source.gov.in",
  "publication_date": null,
  "effective_date": null,
  "language": "Marathi / English",
  "topic": "Topic",
  "procedure_type": "Procedure Type",
  "document_categories": [
    "Category"
  ],
  "content": "Verified description based on the official source.",
  "verified_status": "search_confirmed",
  "source_authority": "Government Authority",
  "retrieved_at": "YYYY-MM-DD",
  "data_origin": "verified_public_source"
}
```

---

## Contributing

When contributing:

1. Do not add duplicate documents.
2. Do not add unofficial blogs as government sources.
3. Do not synthesize legal rules.
4. Preserve the existing schema.
5. Validate all required fields.
6. Include the official source URL.
7. Include the source authority.
8. Include the retrieval date.

---

## Version

```text
Dataset: D4
Version: 1.0
Records: 50
Focus: Maharashtra Government Procedures
Last Retrieved: 2026-09-11
```
