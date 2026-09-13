# Functional Specification Document
## Author AI — AI-Assisted Multi-Chapter Book Creation App (Windows)
### Version 1.0 — Final (Two-Model Strategy)

---

## 1. Document Control

| Field | Value |
|---|---|
| Document Title | Author AI – Functional Specification |
| Version | 1.0 (Final) |
| Platform | Windows 10/11 (Desktop, x64 & ARM64) |
| Status | Approved for Development |
| Author | [Your Name] |
| Last Updated | [Date] |
| Architecture | Local LLM, Two-Model Strategy (Architect + Writer) |

---

## 2. Purpose & Overview

**Author AI** is a Windows desktop application that guides a user through creating a complete multi-chapter book using **locally hosted AI models**. It captures intent, gathers structured input, proposes an editable chapter outline, generates the full manuscript, and provides book review and reporting.

**Core Innovation — The Two-Model Strategy:**
The app uses **two specialized local LLMs** working in tandem:

| Role | Model Class | Responsibility |
|---|---|---|
| **The Architect** | Instruction-tuned reasoning model (e.g., Qwen3 32B class) | Questionnaire handling, outline generation, chapter beat planning, consistency review, reporting |
| **The Writer** | Creative prose model (e.g., Llama 3.3 70B or creative finetune) | Chapter prose generation, style sample, rewrite suggestions |

This decoupling lets moderate hardware still produce a high-quality outline and review, while reserving the heavier creative model for drafting.

**Goal:** Turn a guided Q&A into a finished, review-ready manuscript — entirely on the user's machine, with no cloud dependency required.

---

## 3. Scope

### 3.1 In Scope (v1.0)
- Fiction & Non-Fiction workflows
- Guided questionnaire engine with branching logic
- Writing style selection with live sample preview
- Book volume (Short / Medium / Long)
- AI chapter outline proposal + full editor
- Per-chapter info collection
- Full book generation using two-model pipeline
- Book review (consistency, pacing, style, readability)
- Reporting (session, generation stats, review summary)
- Export (DOCX, PDF, Markdown, EPUB, TXT, CSV)
- Local model management (download, select, assign roles)

### 3.2 Out of Scope (v1.0)
- Multi-user collaboration
- Cloud sync / online accounts
- Publishing marketplace integrations (KDP, etc.)
- Image/illustration generation
- Audio narration
- Series/sequel continuity tracking

---

## 4. Target Users

| Persona | Description | Primary Need |
|---|---|---|
| Aspiring Author | No formal writing background | Structure & drafting help |
| Subject-Matter Expert | Wants to write non-fiction | Fast first draft |
| Hobbyist Novelist | Wants creative assistance | Plot & chapter scaffolding |
| Privacy-Conscious Writer | Won't use cloud AI | Full offline capability |

---

## 5. Assumptions & Dependencies

- User has a machine meeting the **minimum hardware spec** (Section 14).
- Local inference runtime installed or bundled (recommend **Ollama** or **llama.cpp** as the backend).
- Models are downloaded once and stored locally (GGUF format preferred).
- No internet is required after initial model download.
- User has local filesystem access for projects and exports.

---

## 6. High-Level Architecture
