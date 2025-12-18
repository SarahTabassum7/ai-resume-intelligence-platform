# System Architecture Overview

## Purpose

This document explains the architectural design decisions behind the AI-enabled resume intelligence platform.
It complements the high-level architecture diagram in the README by detailing component responsibilities,
design rationale, and trade-offs considered for scalability, security, and accuracy.

The architecture reflects platform-level thinking suitable for real-world recruitment systems rather than
a standalone or experimental implementation.

---

## Architectural Goals

The system was designed to meet the following goals:

- **Scalability:** Handle increasing resume volume and concurrent recruiter usage
- **Explainability:** Ensure AI-assisted outputs can be reviewed and corrected
- **Data Consistency:** Standardize unstructured resume data into a unified schema
- **Security & Privacy:** Protect sensitive candidate information
- **Extensibility:** Support future enhancements without rework

---

## High-Level Architecture Recap

The platform follows a modular, layered architecture consisting of:

- Resume ingestion and storage
- Parsing and standardization pipeline
- AI-assisted enrichment
- Structured candidate profile storage
- Multiple downstream consumers (search, analytics, UI)
- Human-in-the-loop feedback loop

Each component is loosely coupled to allow independent evolution and scaling.

---

## Component Breakdown

### 1. Resume Ingestion & Storage

- Resumes are uploaded through a secure interface
- Files are validated by type and size
- Approved files are stored in **Google Cloud Storage**
- Metadata (filename, upload time, file type) is persisted in the database

**Design choice:**  
Separating file storage from metadata storage reduces database load and improves scalability.

---

### 2. Text Extraction & Parsing

- Uploaded resumes are processed to extract raw text
- PDF and document formats are handled through a parsing pipeline
- Parsing output is normalized to a common intermediate structure

**Trade-off considered:**  
Early normalization simplifies downstream processing but requires handling edge cases (tables, multi-column layouts).

---

### 3. Resume Standardization Layer

This layer transforms parsed text into structured fields such as:

- Education
- Experience
- Dates
- Job titles

**Why this layer exists:**  
Standardization enables consistent analytics, filtering, and comparison across candidates regardless of resume format.

---

### 4. AI-Assisted Enrichment

The platform uses a **hybrid AI approach**:

- Rule-based logic for deterministic fields
- LLM-assisted extraction for unstructured content such as:
  - Skills
  - Role summaries
  - Experience descriptions

Each AI-extracted field includes:
- Confidence score
- Extraction method
- Review flag (if confidence is low)

**Reasoning:**  
This avoids over-reliance on AI while still benefiting from contextual understanding.

---

### 5. Candidate Profile Schema

All extracted and enriched data is persisted in a centralized **Candidate Profile Schema**.
This schema acts as the single source of truth across the platform.

Benefits:
- Enables recruiter search and filtering
- Supports analytics and reporting
- Provides traceability and auditability

Schema details are documented in `docs/data-model.md`.

---

### 6. Recruiter Review Interface (Human-in-the-Loop)

Recruiters can:
- Review AI-extracted fields
- Correct inaccuracies
- Approve profiles for downstream use

Corrections are written back to the candidate profile while preserving audit metadata.

**Why this matters:**  
Human-in-the-loop validation increases trust, improves data quality, and supports responsible AI adoption.

---

### 7. Search, Analytics & Reporting

Structured candidate profiles are consumed by:

- **Search & filtering services** (skills, experience, location)
- **Analytics dashboards** built using Power BI
- Operational and performance reporting

This separation ensures analytics do not interfere with core transactional workflows.

---

## Security & Access Control

- Authentication is handled using **Spring Security with JWT**
- Role-based access controls restrict sensitive operations
- Passwords are hashed using **BCrypt**
- Token invalidation is supported via a custom blacklist service

Sensitive candidate data is protected at rest and in transit.

---

## Why This Architecture Scales

- Modular components allow independent scaling
- Stateless APIs support horizontal expansion
- Asynchronous enrichment prevents blocking user workflows
- Clear separation of concerns reduces system complexity

The design aligns with enterprise-grade recruitment platforms rather than single-purpose tools.

---

## Architectural Trade-offs Summary

| Decision Area | Choice | Rationale |
|-------------|------|-----------|
| AI strategy | Hybrid | Balances accuracy, explainability, and trust |
| Data storage | Normalized schema | Enables analytics and search |
| Validation | Human-in-the-loop | Reduces AI error risk |
| Architecture style | Layered REST | Familiar, scalable, maintainable |

---

## Conclusion

This architecture reflects a deliberate balance between automation and human oversight.
By combining standardized data models, AI-assisted enrichment, and recruiter validation,
the platform supports scalable, transparent, and data-driven hiring workflows.

The design decisions prioritize long-term maintainability, compliance, and trust—key requirements
for real-world recruitment systems.
