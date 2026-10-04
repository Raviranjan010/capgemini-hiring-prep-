[Home](../README.md) > [00-start-here](README.md) > 03_tags-and-how-to-use.md

# Metadata Tags & Navigation Guidelines

Every question, problem, and practice item in this repository is explicitly tagged with metadata to indicate its origin, difficulty, and curriculum categorization.

---

## 1. Content Provenance Tags

| Tag | Formal Definition & Origin | Integrity Rules |
| :---: | :--- | :--- |
| `[VIDEO]` | Directly derived from KN Academy student video debriefs or masterclasses. | Wording and options reflect reported student exam experiences. Never modified without note. |
| `[CHAT]` | Discussed and analyzed within detailed technical debriefs and candidate chat logs. | Verified technical scenarios reported by candidates. |
| `[ADDED]` | Standard technical question authored to complete the core syllabus coverage. | Standard textbook and industry interview problems filling curriculum gaps. |
| `[PATTERN]` | Practice question newly modelled on recent candidate reported patterns. | **Practice question only.** Not an actual past exam question. Models current question mechanics. |
| `[UNVERIFIED]` | Quantitative metrics (timings, scores, cutoffs) reported without official vendor publication. | Documented as commonly reported rules of thumb rather than binding rules. |
| `[OPTIONAL]` | Specialized or peripheral topics that provide deep context but exceed minimum exam scope. | Retained per the Zero-Loss Rule. Recommended for distinction candidates. |

---

## 2. Question Classification Metadata

Every question includes:
- **ID**: Standardized identifier (e.g. `AI-001`, `NET-010`, `DBG-005`, `COG-020`, `MOCK1-015`).
- **Difficulty**: `Easy` | `Medium` | `Hard`.
- **Topic**: Specific knowledge domain (e.g. `Vector Search`, `Subnetting`, `Dynamic Programming`).
- **Source**: Explicit reference back to video URL, chat transcript, or added practice.

---

## 3. Template Structures

### Standard MCQ Template
- **Question**: Contextual scenario, technical snippet, or theoretical problem.
- **Options**: Four mutually exclusive choices (A, B, C, D).
- **Correct Answer**: Bold identifier.
- **Why**: 3 to 5 clear, accessible sentences explaining the underlying mechanism.
- **5-Second Shortcut**: A single memorable takeaway rule.
- **Trap**: Common candidate misconception or distractor analysis.
- **Source**: Verification link or attribution.

### Standard Debugging Template
- **Problem Statement & Sample I/O**: Functional requirement and input/output behavior.
- **Buggy Code**: Syntax or logical implementation containing planted faults.
- **Bugs Found**: Numbered list describing exact line, error mechanism, and correction.
- **Fixed Code**: Complete, production-grade implementation (Java visible; C++ in expandable `<details>`).
- **Dry-Run Table**: Iterative step-by-step state trace.
- **Spot-It-Fast Rule**: Visual code scanner cue.
- **Edge Cases**: Zero, negative, single-element, and boundary handling.
- **Complexity**: Asymptotic Time and Space bounds.

---

Previous: [02_study-plans.md](02_study-plans.md) | Next: [04_syllabus-map.md](04_syllabus-map.md)
