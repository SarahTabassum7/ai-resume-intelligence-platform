# Candidate Profile Data Model

## Purpose

The candidate profile data model defines how unstructured resume data is transformed into a standardized, structured format
that supports recruiter search, analytics, and AI-assisted decision-making. The schema is designed to be extensible,
auditable, and privacy-aware.

This model serves as the canonical representation of a candidate across resume ingestion, enrichment, review, and reporting
workflows.

---

## Design Principles

- **Standardization first:** All resumes map to a consistent schema regardless of original format
- **Searchable structure:** Key attributes are normalized to support filtering and ranking
- **Human-in-the-loop support:** Fields can be corrected or overridden by recruiters
- **Auditability:** Changes are tracked with timestamps and confidence indicators
- **Privacy-aware:** Only required candidate information is retained

---

## High-Level Entity Overview

The core entities involved in the resume intelligence model include:

- Candidate
- Resume
- Experience
- Education
- Skills
- Certifications
- AI Metadata

Each entity is designed to evolve independently without breaking downstream analytics or search functionality.

---

## Candidate Profile (Core Entity)

| Field | Type | Description |
|------|------|-------------|
| candidateId | UUID | Unique identifier for the candidate |
| firstName | String | Candidate first name |
| lastName | String | Candidate last name |
| email | String | Contact email |
| phone | String | Contact phone number |
| location | String | City, state, or region |
| totalExperienceYears | Decimal | Calculated total professional experience |
| currentTitle | String | Most recent job title |
| profileStatus | Enum | Draft / Reviewed / Approved |
| createdAt | Timestamp | Profile creation time |
| updatedAt | Timestamp | Last update time |

---

## Resume Metadata

| Field | Type | Description |
|------|------|-------------|
| resumeId | UUID | Unique resume identifier |
| fileName | String | Uploaded resume filename |
| fileType | Enum | PDF / DOC / DOCX |
| storagePath | String | Secure cloud storage location |
| uploadedAt | Timestamp | Resume upload time |
| parsedAt | Timestamp | Resume parsing completion time |

---

## Experience Entity

| Field | Type | Description |
|------|------|-------------|
| experienceId | UUID | Unique experience record |
| companyName | String | Employer name |
| jobTitle | String | Role title |
| startDate | Date | Employment start date |
| endDate | Date | Employment end date or null |
| responsibilities | Text | Role responsibilities |
| isCurrent | Boolean | Indicates current role |

---

## Education Entity

| Field | Type | Description |
|------|------|-------------|
| educationId | UUID | Education record identifier |
| institution | String | University or institution |
| degree | String | Degree earned |
| fieldOfStudy | String | Major or specialization |
| graduationYear | Integer | Year of completion |

---

## Skills Entity

| Field | Type | Description |
|------|------|-------------|
| skillId | UUID | Skill identifier |
| skillName | String | Skill name |
| skillCategory | String | Technical / Functional / Soft |
| proficiencyLevel | Enum | Beginner / Intermediate / Advanced |
| extractedByAI | Boolean | AI-extracted or manually added |

---

## Certifications Entity

| Field | Type | Description |
|------|------|-------------|
| certificationId | UUID | Certification identifier |
| certificationName | String | Certification title |
| issuingOrganization | String | Issuing body |
| issueYear | Integer | Year issued |

---

## AI Extraction Metadata

| Field | Type | Description |
|------|------|-------------|
| extractionConfidence | Decimal | Confidence score (0–1) |
| extractionMethod | Enum | Rule-based / LLM-assisted |
| needsReview | Boolean | Flag for recruiter validation |
| lastReviewedBy | String | Recruiter identifier |
| lastReviewedAt | Timestamp | Review timestamp |

---

## Human-in-the-Loop Updates

Recruiter edits and corrections update the candidate profile while preserving original extracted values through audit logs.
This ensures transparency, accuracy, and continuous improvement of extraction quality.

---

## Why This Model Matters

- Enables **consistent candidate comparison**
- Supports **search, filtering, and ranking**
- Provides a foundation for **analytics and reporting**
- Improves **trust in AI-assisted hiring workflows**
- Scales across roles, industries, and hiring volumes

---

## Notes on Data Privacy

All example data used in this repository is synthetic. No real candidate information is stored or exposed.
Sensitive fields are protected through role-based access and audit logging.
