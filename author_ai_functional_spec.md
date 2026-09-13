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

<img width="328" height="337" alt="image" src="https://github.com/user-attachments/assets/b4910cf3-ea9c-4951-9454-2e5d92da040a" />





---

## 7. High-Level User Flow

<img width="375" height="521" alt="image" src="https://github.com/user-attachments/assets/a2db75bd-2e4b-4cdb-8fa3-5c7901274a84" />




---

## 8. Functional Requirements

### FR-1 Project Management
- **FR-1.1** Create new project (title, author name, save location).
- **FR-1.2** Open existing project (`.authai` project folder).
- **FR-1.3** Save / Save As / Auto-save every 60 seconds.
- **FR-1.4** Recent projects list on home screen.
- **FR-1.5** Delete / archive project.
- **FR-1.6** Project versioning (snapshot per generation run).

### FR-2 Book Type Selection
- **FR-2.1** Present **Fiction** and **Non-Fiction** as primary choices.
- **FR-2.2** Optional sub-genres:
  - **Fiction:** Fantasy, Sci-Fi, Romance, Thriller, Mystery, Literary, Historical, YA, Children's.
  - **Non-Fiction:** Self-Help, Business, Memoir, Technical, History, Science, Biography, How-To.
- **FR-2.3** Selection drives the questionnaire branch (FR-3).

### FR-3 Guided Questionnaire *(Architect)*
- **FR-3.1** One question per screen with progress indicator.
- **FR-3.2** Support free-text, single-select, multi-select, numeric inputs.
- **FR-3.3** Incremental save; Back / Skip / Save & Exit.
- **FR-3.4** **Fiction set:**
  - Premise / logline
  - Setting (time, place, world rules)
  - Protagonist (name, goal, flaw)
  - Antagonist / conflict
  - Supporting characters
  - Central theme
  - Tone (dark, hopeful, humorous…)
  - Point of view (1st/3rd, limited/omniscient)
  - Target audience / age range
  - Ending preference (happy, bittersweet, open)
- **FR-3.5** **Non-Fiction set:**
  - Topic / thesis
  - Target reader
  - Author's credentials / perspective
  - Key takeaways (3–7)
  - Research sources / references
  - Tone (academic, conversational, motivational)
  - Structure preference (chronological, thematic, problem-solution)
  - Call to action
- **FR-3.6** Architect may suggest follow-up clarifying questions when answers are ambiguous.
- **FR-3.7** Full answer review/edit screen before proceeding.

### FR-4 Writing Style Selection *(Writer)*
- **FR-4.1** Predefined styles:
  - **Fiction:** Literary, Commercial, Minimalist, Descriptive, Cinematic, Humorous, Dark/Gritty, Whimsical.
  - **Non-Fiction:** Academic, Conversational, Journalistic, Motivational, Instructional, Narrative.
- **FR-4.2** **Custom Style** free-text field.
- **FR-4.3** Writer generates a **live sample paragraph** (~150 words) for confirmation.
- **FR-4.4** Regenerate sample up to 5 times before locking.
- **FR-4.5** If sample rejected 5×, prompt for Custom Style.
- **FR-4.6** Selected style is stored as a **Style Card** (tone, POV, vocabulary level, sentence rhythm) and injected into every Writer prompt.

### FR-5 Book Volume Selection
- **FR-5.1** Three options with estimates:

| Option | Chapters | Words/Chapter | Total Words |
|---|---|---|---|
| Short | 5–7 | 1,500–2,500 | 10,000–15,000 |
| Medium | 10–15 | 2,500–4,000 | 30,000–50,000 |
| Long | 18–30 | 3,500–6,000 | 70,000–120,000 |

- **FR-5.2** Advanced custom override (chapter count + target words).
- **FR-5.3** Display estimated generation time based on measured model speed.

### FR-6 Chapter Outline Proposal *(Architect)*
- **FR-6.1** Architect generates chapter list with:
  - Chapter number & title
  - 2–4 sentence summary
  - Key beats / sections
  - Estimated word count
- **FR-6.2** User actions: **Accept**, **Edit**, **Reorder** (drag-drop), **Add**, **Delete**, **Regenerate chapter**, **Regenerate all**.
- **FR-6.3** Validation: total estimated words within ±10% of selected volume; warn if not.
- **FR-6.4** Lock outline before generation (unlockable).

### FR-7 Chapter Information Collection
- **FR-7.1** Per-chapter optional fields:
  - Additional notes / must-include points
  - Characters / sections to feature
  - Chapter-specific tone adjustments
- **FR-7.2** **Auto-fill from outline** mode if user skips detail entry.
- **FR-7.3** Per-chapter status: Not started / In progress / Ready.

### FR-8 Book Generation *(Two-Model Pipeline)*
- **FR-8.1** Generation runs sequentially, chapter-by-chapter.
- **FR-8.2** **Per-chapter pipeline:**
  1. **Architect** compiles a *Chapter Brief*: outline beats, character states, prior-chapter summary, tone constraints, target word count.
  2. **Writer** generates prose from the Chapter Brief + Style Card.
  3. **Architect** performs a quick *continuity pass*: verifies names, facts, timeline; returns a summary used as context for the next chapter.
  4. Chapter is saved immediately to disk.
- **FR-8.3** Progress UI: current chapter, % complete, ETA, Pause / Cancel.
- **FR-8.4** Failed chapter → retry without restarting the book.
- **FR-8.5** Allow **regenerate chapter** or **inline edit** post-generation.
- **FR-8.6** Enforce style, POV, and length constraints per chapter.
- **FR-8.7** Final compile: title page, TOC, chapters, optional author bio.
- **FR-8.8** Manual **model hot-swap** allowed mid-book (user may switch Writer model between chapters).

### FR-9 Book Review *(Architect)*
- **FR-9.1** Review pass produces:
  - Consistency (characters, timeline, facts)
  - Style/tone adherence score
  - Pacing & structure feedback
  - Repetition / cliché detection
  - Readability score (Flesch-Kincaid)
  - Grammar / spelling flags
  - Strengths & improvement suggestions
- **FR-9.2** Findings presented in a report with **jump-to-location** links.
- **FR-9.3** Per-finding actions: **Accept fix** (Writer rewrites section), **Dismiss**, **Annotate**.
- **FR-9.4** Manual per-chapter annotations supported.

### FR-10 Reporting
- **FR-10.1** Project report includes:
  - Project metadata (title, author, type, genre, style, volume)
  - Questionnaire answers summary
  - Chapter list with word counts & status
  - Generation stats (time, tokens, retries, model used per chapter)
  - Review results summary
  - Version history
- **FR-10.2** Export as **PDF**, **DOCX**, **Markdown**, **CSV**.
- **FR-10.3** Dashboard: total words, chapters complete, review score, avg generation speed.

### FR-11 Export & Output
- **FR-11.1** Manuscript export: **DOCX**, **PDF**, **Markdown**, **EPUB**, **TXT**.
- **FR-11.2** Include/exclude: title page, TOC, chapter numbers, author bio.
- **FR-11.3** Report exported separately (FR-10.2).

### FR-12 Local Model Management
- **FR-12.1** **Model Library** screen listing installed models with size, quantization, and role assignment.
- **FR-12.2** **Role Assignment:**
  - Architect slot (default: Qwen3 32B Q4_K_M)
  - Writer slot (default: Llama 3.3 70B Q4_K_M, or a creative finetune)
- **FR-12.3** Built-in **model downloader** (from Hugging Face / Ollama registry) with progress and checksum verification.
- **FR-12.4** **Hardware auto-detect:** recommend models based on VRAM/RAM.
- **FR-12.5** Warn if selected model exceeds available VRAM (offer CPU offload).
- **FR-12.6** Model integrity check on load.
- **FR-12.7** Option to run models on **CPU only** (slower, minimum spec fallback).

### FR-13 Settings
- **FR-13.1** Runtime selection (Ollama / llama.cpp / bundled).
- **FR-13.2** Inference parameters: temperature, top-p, context length per role.
- **FR-13.3** Default language & locale.
- **FR-13.4** Theme (light/dark), font size.
- **FR-13.5** Auto-save interval.
- **FR-13.6** GPU/CPU thread allocation.

---

## 9. Non-Functional Requirements

| ID | Requirement |
|---|---|
| NFR-1 | **Performance:** UI responds <200 ms; chapter prose streams token-by-token. |
| NFR-2 | **Reliability:** Auto-recovery of in-progress projects after crash. |
| NFR-3 | **Security:** All content stays local. No telemetry by default. Opt-in anonymous usage stats only. |
| NFR-4 | **Privacy:** Zero network calls after model download unless user initiates. |
| NFR-5 | **Usability:** WCAG 2.1 AA; keyboard navigable; screen-reader friendly. |
| NFR-6 | **Scalability:** Handle books up to 150,000 words without UI degradation. |
| NFR-7 | **Compatibility:** Windows 10 (1909+) and Windows 11; x64 & ARM64. |
| NFR-8 | **Localization-ready:** All strings externalized (i18n). |
| NFR-9 | **Offline:** Fully functional offline after models are installed. |
| NFR-10 | **Model swap tolerance:** Changing models does not corrupt in-progress projects. |
| NFR-11 | **Resource safety:** App throttles inference to avoid OS-level freezes on low-RAM machines. |

---

## 10. Data Model (Conceptual)

Project
├─ id, title, author, type (Fiction/NonFiction), genre
├─ style (StyleCard), volume, createdAt, updatedAt
├─ modelConfig { architectId, writerId }
├─ QuestionnaireAnswers { key: value }
├─ Outline [ ChapterPlan ]
├─ Chapters [ ChapterContent ]
├─ Review { findings[], score }
├─ Reports [ Report ]
└─ Versions [ Snapshot ]

StyleCard
├─ name, tone, pov, vocabularyLevel, sentenceRhythm, customNotes

ChapterPlan
├─ number, title, summary, beats[], notes, targetWords, status

ChapterContent
├─ chapterRef, content, actualWords, modelUsed, generatedAt, version

ModelRef
├─ id, name, path, quantization, sizeBytes, role, checksum



---

## 11. UI / Screen Inventory

1. **Home / Project Hub** — New, Open, Recent.
2. **Model Setup / Library** — role assignment, downloads, hardware status.
3. **Book Type & Genre**.
4. **Questionnaire Wizard**.
5. **Style Selection** (+ live sample preview).
6. **Volume Selection**.
7. **Outline Editor** (drag-and-drop).
8. **Chapter Info Collector**.
9. **Generation Progress** view.
10. **Manuscript Editor**.
11. **Review Dashboard**.
12. **Reports & Export**.
13. **Settings**.

---

## 12. Error Handling & Edge Cases

| Scenario | Handling |
|---|---|
| Missing model file | Prompt to download or reassign role |
| Model exceeds VRAM | Warn; offer CPU offload or swap |
| Runtime crash mid-generation | Auto-save partial; resume on relaunch |
| Network failure during model download | Retry with resume support |
| User cancels generation | Save partial manuscript |
| Style sample rejected 5× | Force Custom Style entry |
| Outline word count mismatch | Warning; allow override |
| Context limit exceeded (long books) | Rolling summary via Architect |
| Writer produces off-style prose | Architect continuity pass flags; user can regenerate |
| Low RAM during generation | Throttle inference; notify user |

---

## 13. Acceptance Criteria

- [ ] App detects hardware and recommends Architect + Writer models.
- [ ] User can download, assign, and swap both models locally.
- [ ] Fiction and Non-Fiction flows both complete end-to-end offline.
- [ ] Style sample is generated by the Writer model and matches selection.
- [ ] Outline is editable, reorderable, and total word count matches volume ±10%.
- [ ] Full book generates chapter-by-chapter with no data loss across crashes.
- [ ] Each chapter records which model generated it.
- [ ] Review report lists at least 7 categories of feedback.
- [ ] Report exports successfully to PDF and DOCX.
- [ ] Manuscript exports successfully to DOCX, PDF, Markdown, EPUB, TXT.
- [ ] No network calls occur after models are installed (verified via network monitor).
- [ ] Model hot-swap mid-book does not corrupt the project.

---

## 14. Hardware Tiers & Model Recommendations

| Tier | Hardware | Architect Model | Writer Model | Expected Quality |
|---|---|---|---|---|
| **Minimum** | 8 GB VRAM / 16 GB RAM | Qwen3 8B class | Llama 3.1 8B | Draft-quality; expect heavy editing |
| **Recommended** | 16–24 GB VRAM | Qwen3 32B (Q4) | Mistral Small 24B / Gemma 3 27B | Solid, structured, good prose |
| **Ideal** | 48 GB+ VRAM / 64 GB+ Unified | Qwen3 32B (Q5/Q6) | Llama 3.3 70B (Q4) | Publication-quality first draft |
| **Enthusiast** | Dual GPU / 128 GB Unified | Qwen3 32B (FP16) | Llama 3.3 70B (Q6/Q8) or creative finetune | Near-human prose quality |

**Default out-of-box configuration:** Recommended tier (Qwen3 32B + Llama 3.3 70B Q4), auto-downgraded if hardware insufficient.

---

## 15. Future Enhancements (Post-v1.0)

- Cover design generator (local Stable Diffusion)
- Multi-language output
- Collaborative editing over LAN
- Version diff / branching
- Direct publishing integrations
- Voice narration export (local TTS)
- Series / sequel continuity tracking
- Additional specialized models per genre (romance, thriller, technical)

---

**End of Specification — Version 1.0 Final**
