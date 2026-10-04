[Home](../README.md) > [07-communication](README.md) > 01_comprehensive_english_modules.md

# Capgemini English & Communication Assessment — Comprehensive 6-Module Guide

> **Source Analysis**: Based on the comprehensive Capgemini hiring preparation video (*KN ACADEMY: Complete Capgemini One Shot Preparation | Capgemini Technical, Cognitive, English Test | Solution* — Video Reference: [Complete Capgemini Master Preparation Video](https://www.youtube.com/watch?v=q5giVVUApwM)).

---

## Complete Exam Architecture & Round Overview

Capgemini has revamped its hiring framework, replacing generic aptitude quizzes with an enterprise-grade evaluation pipeline:

```text
Capgemini Comprehensive Assessment Flow
├── Round 1: English & Communication Assessment (6 Modules)
├── Round 2: Code Debugging Assessment (20 Minutes)
├── Round 3: AI-Assisted (Vibe) Coding Assessment (20 Minutes)
└── Round 4: Cognitive & Game-Based Assessment (4 Modules)
```

---

## Round 1: English & Communication Assessment (6 Modules)

The communication round evaluates written business judgment, reading comprehension, auditory accuracy, spoken fluency, proposal structuring, and grammatical mechanics across six dedicated modules.

---

### Module 1 (COM-001): Situational Awareness & Workplace Escalation (6 Questions)

#### The Scenario Prompt
> An enterprise client has sent a critical, high-severity email expressing extreme frustration regarding an ongoing 48-hour downtime in the staging environment that has halted and delayed their internal sprint testing schedule. Your Project Lead is out of the office without access to email.  
> **Task**: Write a formal email response acknowledging the severity of the issue, reassuring them of an ongoing investigation, and proposing an updated timeline without over-promising or making unauthorized technical commitments.

#### Step-by-Step Response Strategy
1. **Emotional De-escalation**: Acknowledge their frustration immediately and validate the business impact without sounding defensive or evasive.
2. **Actionable Reassurance**: Clearly communicate that the technical and DevOps teams have already mobilized to investigate, even while project leadership is temporarily unreachable.
3. **Realistic Commitment (No Over-Promising)**: Do not guarantee an immediate total resolution. Instead, commit to a strict, measurable milestone (e.g., an investigation report and revised timeline by 3:00 PM today).

#### Full Model Response
```text
Subject: RE: Critical Update: Staging Environment Downtime & Investigation

Dear [Client Name],

Thank you for reaching out to us. We fully understand the frustration caused by the recent staging environment downtime, and we recognize how critical this testing phase is for your internal sprint timeline.

While our Project Lead is currently out of office, our technical infrastructure and DevOps teams have already mobilized to investigate the root cause of the interruption. We are actively working to restore environment stability and implement safeguards to prevent this from happening again.

I will monitor this issue personally and share a comprehensive incident update, along with a revised testing timeline, by 3:00 PM today.

Thank you for your patience as we work through this.

Sincerely,  
[Your Name]  
Associate Project Consultant, Capgemini
```

---

### Module 2: Reading Comprehension (4 Questions)

#### Sample Technical Passage
> *"The enterprise transition to cloud-native architectures offers elasticity, enabling organizations to scale compute resources dynamically in response to volatile workload demands. However, decoupling monolithic systems into distributed microservices introduces operational overhead. Managing asynchronous network latency, distributed state, and data consistency across disparate clusters requires robust observability platforms and automated CI/CD deployment pipelines."*

#### Key Questions & Explanations

##### Question 1 (COM-002): Operational Trade-offs
**Question**: What is the primary operational trade-off of shifting from a monolith to microservices based on the text?  
**Correct Answer**: Increased operational complexity in managing distributed state, asynchronous latency, and data consistency across clusters.  
**Explanation**: While microservices increase agility, the passage explicitly identifies distributed state, asynchronous network latency, and cross-cluster consistency as primary operational overheads.

##### Question 2 (COM-003): Cloud-Native Elasticity
**Question**: How does cloud-native elasticity benefit enterprise computing?  
**Correct Answer**: It allows systems to dynamically provision and scale compute resources up or down to handle volatile traffic demands.  
**Explanation**: The text states elasticity enables organizations to *"scale compute resources dynamically in response to volatile workload demands."*

---

### Module 3 (COM-004): Listening Comprehension (4 Questions)

#### Audio Transcript
*(Spoken via audio prompt once)*
> *"Attention team members: The mandatory quarterly security compliance audit has been rescheduled from Wednesday morning to Thursday at 3:00 PM. Please ensure that all code repositories are scanned and access logs updated by Wednesday evening."*

#### The Assessment Question
**Question**: By when must team members complete scanning their code repositories and updating access logs?
- **A)** Thursday at 3:00 PM
- **B)** Wednesday morning
- **C)** Wednesday evening
- **D)** Friday morning

**Correct Answer**: **C (Wednesday evening)**

> [!WARNING]
> **Exam Trap**: **Thursday at 3:00 PM** is when the rescheduled audit begins. The preparation deadline for scanning repositories and updating access logs is strictly **Wednesday evening**. Do not confuse the event start time with the preparation deadline.

---

### Module 4 (COM-005): Spoken Technical Delivery (2 Topics)

#### Format & Timing
- **Preparation Window**: 60 Seconds
- **Speech Recording Window**: 90 Seconds (uninterrupted microphone capture)

#### Topic Prompt
> *"Discuss the impact of Generative AI on software development practices. Structure your response to cover: (1) Potential productivity gains, (2) Quality assurance considerations, and (3) Ethical implications regarding proprietary intellectual property."*

#### 90-Second Speech Blueprint

```text
Time Allocation & Talking Points
├── 0:00 - 0:25 (Productivity):
│   └── Boilerplate generation, regex construction, and automated unit test scaffolding.
├── 0:25 - 0:55 (Quality Assurance):
│   └── Hallucinations, subtle logic bugs, and why human-in-the-loop review is mandatory.
└── 0:55 - 1:30 (Ethics & IP Governance):
    └── PII leaks, copyright infringement in training sets, and enterprise data sandboxing.
```

#### Full Model Spoken Response
> *"Good day. Artificial Intelligence is fundamentally transforming software engineering across productivity, code quality, and technical ethics.*  
>  
> *First, regarding productivity, Generative AI acts as an intelligent co-pilot. It accelerates development by automating boilerplate code, generating complex regex patterns, and drafting baseline unit tests. This frees engineers to focus on higher-level architectural design.*  
>  
> *However, from a quality assurance perspective, AI-generated code introduces unique risks. Large language models operate probabilistically, frequently generating subtle logic errors or non-existent API methods. Therefore, automated static analysis and rigorous human-in-the-loop code reviews remain non-negotiable to maintain enterprise stability.*  
>  
> *Finally, regarding ethics and intellectual property, organizations must enforce strict governance. Ingesting proprietary client source code into public LLMs poses severe data leakage and copyright risks. Enterprises must implement private, air-gapped models and sandboxed environments to safeguard trade secrets.*  
>  
> *In summary, while AI significantly amplifies developer velocity, its long-term success relies entirely on diligent verification and strict security governance. Thank you."*

---

### Module 5 (COM-006): Business Proposal Writing (2 Questions)

#### The Prompt
> Write a formal business proposal email to your Department Head requesting budget approval for **$2,500** to integrate an AI-powered code security scanner into your team's CI/CD pipeline. Address the expected ROI, security benefits, and implementation timeline.

#### Model Business Proposal Email
```text
Subject: Proposal: Budget Request for AI-Powered CI/CD Security Integration ($2,500)

Dear [Department Head Name],

I am writing to propose the integration of an AI-powered code security scanner into our CI/CD deployment pipeline and request a one-time budget approval of $2,500.

Key Business & Security Benefits:
1. Automated Vulnerability Detection: Scans pull requests in real time to catch dependency vulnerabilities, memory leaks, and hardcoded secrets before they reach staging.
2. Measurable ROI: By shifting security checks left, we reduce post-release patches, saving an estimated 15 developer hours per sprint.

Implementation Timeline:
- Week 1: Environment configuration and pilot branch integration.
- Week 2: Baseline rules tuning and false-positive suppression.
- Week 3: Full CI/CD production rollout across all services.

I have attached the complete vendor evaluation for your review and look forward to discussing this in our weekly sync.

Best regards,  
[Your Name]  
DevOps Engineering Team, Capgemini
```

---

### Module 6: Grammar & Sentence Correction (10 Questions)

#### High-Frequency Practice Questions & Rules

##### Question 1 (COM-007): Subject-Verb Agreement (Proximity Rule)
> *"Neither the lead architect nor the backend engineers (was / were) able to isolate the deadlock condition."*

- **Rule**: In correlative conjunctions (*Neither ... nor*, *Either ... or*), the verb must agree with the subject **closest** to it.
- **Analysis**: The closer subject is *"backend engineers"* (plural).
- **Correct Answer**: **were**  
- **Full Sentence**: *"Neither the lead architect nor the backend engineers were able to isolate the deadlock condition."*

##### Question 2 (COM-008): Dangling Modifier Correction
> **Buggy Sentence**: *"Having executed the database migration, the memory spike was observed by the DevOps team."*

- **Grammar Rule**: A introductory participial phrase (*"Having executed..."*) must modify the grammatical subject immediately following the comma. As written, it implies the *memory spike* executed the migration.
- **Correction**: Place the logical agent (*the DevOps team*) immediately after the comma.
- **Corrected Sentence**: *"Having executed the database migration, the DevOps team observed the memory spike."*

## Section 9: Practice Prompts & Exam Simulation (30 Prompts)

### COM-009: Grammar: Subject-Verb Agreement with 'Neither/Nor'

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Grammar Mechanics

#### Prompt / Question
Identify the grammatically correct option to fill the blank:
'Neither the project manager nor the software developers ________ willing to compromise on code quality.'

- **A**: was
- **B**: were
- **C**: is
- **D**: are being

**Correct Answer**: **B**

#### Why
When subjects are joined by 'neither...nor', the verb agrees with the closer subject. 'Software developers' is plural, requiring the plural verb 'were'.

- **Exam Tip**: In 'neither/nor' constructions, match the verb to the subject nearest to it.
- **Source**: Added practice

---
### COM-010: Vocabulary: Corporate Context Synonyms

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Vocabulary

#### Prompt / Question
Select the word most similar in meaning to **PRAGMATIC** in an enterprise delivery context:
'The team took a pragmatic approach to meet the release deadline.'

- **A**: Idealistic
- **B**: Practical
- **C**: Theoretical
- **D**: Reckless

**Correct Answer**: **B**

#### Why
'Pragmatic' means dealing with things sensibly and realistically in a way that is based on practical rather than theoretical considerations.

- **Exam Tip**: Pragmatic = Practical / Results-oriented.
- **Source**: Added practice

---
### COM-011: Error Spotting: Misplaced Modifier

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Sentence Correction

#### Prompt / Question
Which underlined part of the sentence contains an error?
'Having compiled the code (A) / without errors, (B) / the testing suite was run by Priya. (C) / No error (D)'

- **A**: Part A
- **B**: Part B
- **C**: Part C
- **D**: Part D

**Correct Answer**: **C**

#### Why
The introductory participial phrase 'Having compiled the code without errors' must modify the subject performing the action (Priya). As written, it improperly modifies 'the testing suite'. It should read: '...Priya ran the testing suite.'

- **Exam Tip**: A participial phrase at the start must be immediately followed by the person/agent who performed it.
- **Source**: Added practice

---
### COM-012: Sentence Rearrangement: Logical Workflow

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Parajumbles

#### Prompt / Question
Arrange the sentences in the most logical sequence:
P. The deployment pipeline executed unit and integration test suites.
Q. Developers submitted pull requests for peer code review.
R. The approved release bundle was pushed to production servers.
S. Code reviews identified boundary condition edge cases.

- **A**: Q - S - P - R
- **B**: P - Q - R - S
- **C**: Q - P - S - R
- **D**: S - Q - P - R

**Correct Answer**: **A**

#### Why
Chronological workflow: First developers submit PRs (Q), reviews find bugs (S), pipeline runs tests (P), and finally the bundle is released to production (R).

- **Exam Tip**: Look for chronological software development lifecycle steps: Code -> Review -> Test -> Deploy.
- **Source**: Added practice

---
### COM-013: Grammar: Tense Consistency in Reported Speech

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Grammar Mechanics

#### Prompt / Question
Select the correct indirect speech conversion:
'She said, "I have completed the API documentation."'

- **A**: She said that she has completed the API documentation.
- **B**: She said that she had completed the API documentation.
- **C**: She said that she completed the API documentation.
- **D**: She said that she was completing the API documentation.

**Correct Answer**: **B**

#### Why
In indirect speech with a past reporting verb ('said'), Present Perfect ('have completed') shifts back to Past Perfect ('had completed').

- **Exam Tip**: Present Perfect becomes Past Perfect when the reporting verb is in past tense.
- **Source**: Added practice

---
### COM-014: Vocabulary: Professional Corporate Antonyms

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Vocabulary

#### Prompt / Question
Choose the word most opposite in meaning to **OBSOLETE**:
'Legacy monolithic architectures are becoming obsolete.'

- **A**: Outdated
- **B**: Contemporary
- **C**: Redundant
- **D**: Antique

**Correct Answer**: **B**

#### Why
'Obsolete' means no longer produced or used; out of date. 'Contemporary' means current, modern, or belonging to the present time.

- **Exam Tip**: Obsolete = Outdated; Antonym = Modern / Contemporary.
- **Source**: Added practice

---
### COM-015: Sentence Completion: Idiomatic Business Prepositions

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Fill in Blanks

#### Prompt / Question
Choose the correct preposition:
'The cloud migration project was completed ________ accordance with enterprise compliance guidelines.'

- **A**: in
- **B**: with
- **C**: on
- **D**: by

**Correct Answer**: **A**

#### Why
The standard formal English idiom is 'in accordance with'.

- **Exam Tip**: Memorize set phrases: 'in accordance with', 'with respect to', 'in compliance with'.
- **Source**: Added practice

---
### COM-016: Extempore Prompt: Cloud vs On-Premise Speaking Blueprint

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Spoken English

#### Prompt / Question
In a 45-second extempore on 'Cloud Migration vs On-Premise Servers', which 3-point structure scores highest on automated speech metrics?

- **A**: Definition (10s) -> Business Benefits (Scalability & Cost) (25s) -> Balanced Conclusion (10s)
- **B**: Speaking continuously without pausing for breath
- **C**: Listing 20 technical acronyms quickly
- **D**: Telling a humorous personal story

**Correct Answer**: **A**

#### Why
Automated evaluation engines evaluate structural coherence, clarity of articulation, topical vocabulary, and pacing. A 3-point framework (Context -> Evidence -> Conclusion) ensures smooth flow without hesitation.

- **Exam Tip**: Use PREP formula: Point -> Reason -> Example -> Point.
- **Source**: Added practice

---
### COM-017: Error Spotting: Parallelism in Technical Writing

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Sentence Correction

#### Prompt / Question
Which sentence demonstrates proper grammatical parallelism?

- **A**: The microservice is responsible for parsing logs, validating tokens, and to send alerts.
- **B**: The microservice is responsible for parsing logs, validating tokens, and sending alerts.
- **C**: The microservice is responsible for parse logs, validate tokens, and send alerts.
- **D**: The microservice is responsible to parse logs, validating tokens, and sends alerts.

**Correct Answer**: **B**

#### Why
Elements in a list governed by a preposition ('for') must maintain identical grammatical forms: gerunds ('parsing', 'validating', 'sending').

- **Exam Tip**: Keep list items parallel in form (all -ing verbs or all base infinitives).
- **Source**: Added practice

---
### COM-018: Sentence Completion: Conditional Clauses (Third Conditional)

**Tag**: [PATTERN] | **Difficulty**: Hard | **Topic**: Grammar Mechanics

#### Prompt / Question
Select the correct verb phrase:
'If the QA team ________ the regression suite earlier, the defect ________ caught before release.'

- **A**: ran, would be
- **B**: had run, would have been
- **C**: have run, will have been
- **D**: would run, had been

**Correct Answer**: **B**

#### Why
Third conditional for past hypothetical events: 'If + past perfect (had run) ..., would have + past participle (would have been)'.

- **Exam Tip**: Third conditional formula: If had + V3 ..., would have + V3.
- **Source**: Added practice

---
### COM-019: Vocabulary: Contextual Word Substitution

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Vocabulary

#### Prompt / Question
Select the most appropriate technical word:
'The distributed caching layer helped ________ database bottleneck latency during peak traffic.'

- **A**: aggravate
- **B**: alleviate
- **C**: exacerbate
- **D**: prolong

**Correct Answer**: **B**

#### Why
'Alleviate' means to make a problem or suffering less severe. Aggravate and exacerbate mean to make worse.

- **Exam Tip**: Alleviate = Relieve / Reduce; Exacerbate = Worsen.
- **Source**: Added practice

---
### COM-020: Reading Comprehension: Inference vs Explicit Fact

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Comprehension

#### Prompt / Question
Passage: 'While automated unit testing catches syntactical and algorithmic faults, it rarely detects subtle asynchronous race conditions that emerge under concurrent production loads.'
Question: What does the passage logically imply?

- **A**: Unit testing is completely useless in production.
- **B**: Load and stress testing are necessary complements to unit testing for distributed systems.
- **C**: Asynchronous code should never be tested with unit tests.
- **D**: Concurrent production loads prevent software bugs.

**Correct Answer**: **B**

#### Why
The author notes unit testing fails to catch race conditions under load, implying higher-level concurrency/load testing is required alongside unit testing.

- **Exam Tip**: Inference questions require deducing unstated implications without inventing extreme claims.
- **Source**: Added practice

---
### COM-021: Corporate Email Etiquette: Professional Tone

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Business Communication

#### Prompt / Question
Which phrasing is most appropriate for a client status update email regarding a slight delay?

- **A**: We won't make the deadline because the database crashed.
- **B**: Due to unexpected database latency during final integration, the release will be deployed tomorrow at 10 AM following verification.
- **C**: Sorry, you will have to wait until tomorrow.
- **D**: It's not our fault, the third-party provider caused a delay.

**Correct Answer**: **B**

#### Why
Professional communication acknowledges the root cause objectively, presents a clear mitigation timeline, and avoids defensive or colloquial blame.

- **Exam Tip**: Professional tone: State fact -> State mitigation -> State revised timeline.
- **Source**: Added practice

---
### COM-022: Jumbled Sentence Reconstruction

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Sentence Structure

#### Prompt / Question
Reconstruct the words into a grammatically coherent sentence:
'1: architecture / 2: enhances / 3: microservices / 4: scalability / 5: system'

- **A**: 3 - 1 - 2 - 5 - 4
- **B**: 1 - 3 - 2 - 4 - 5
- **C**: 5 - 4 - 2 - 3 - 1
- **D**: 3 - 4 - 2 - 5 - 1

**Correct Answer**: **A**

#### Why
'Microservices architecture enhances system scalability' (3 - 1 - 2 - 5 - 4) follows Subject (3, 1) + Verb (2) + Object (5, 4).

- **Exam Tip**: Identify Subject (Microservices architecture) -> Verb (enhances) -> Object (system scalability).
- **Source**: Added practice

---
### COM-023: Grammar: Active vs Passive Voice in Documentation

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Grammar Mechanics

#### Prompt / Question
Which sentence correctly converts this active statement to passive voice?
'The DevOps engineer deployed the container image.'

- **A**: The container image was deployed by the DevOps engineer.
- **B**: The container image had been deployed by the DevOps engineer.
- **C**: The container image deployed the DevOps engineer.
- **D**: The container image is being deployed by the DevOps engineer.

**Correct Answer**: **A**

#### Why
Simple past active ('deployed') converts to simple past passive ('was deployed').

- **Exam Tip**: Simple Past active (V2) becomes 'was/were + V3' in passive.
- **Source**: Added practice

---
### COM-024: Speech Pacing & Acoustic Metric Heuristic

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Spoken English

#### Prompt / Question
What is the recommended speaking rate for automated assessment engines (e.g. Versant / SVAR)?

- **A**: Above 220 words per minute to prove high fluency
- **B**: 120 to 150 words per minute with clear pauses between clauses
- **C**: Below 70 words per minute with prolonged pauses
- **D**: Monotone flat pitch with no syllable stress

**Correct Answer**: **B**

#### Why
Speech-to-text engines require a natural, steady conversational pace of 120-150 WPM with distinct clause articulation. Rushing produces word truncation and lower acoustic scores.

- **Exam Tip**: Speak at 120-150 WPM; never rush to beat the clock.
- **Source**: Added practice

---
### COM-025: Vocabulary: Precision in Technical Status Reporting

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Vocabulary

#### Prompt / Question
Select the best word:
'The performance benchmarks were ________ to those achieved in the previous benchmark cycle.'

- **A**: comparable
- **B**: comparative
- **C**: comparing
- **D**: comparably

**Correct Answer**: **A**

#### Why
'Comparable to' is the standard adjective meaning similar or of equivalent quality.

- **Exam Tip**: 'Comparable to' = similar in quality or degree.
- **Source**: Added practice

---
### COM-026: Error Spotting: Double Negatives in Business Writing

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Sentence Correction

#### Prompt / Question
Identify the error in this statement:
'The senior developer hardly knew nothing (A) / about the legacy codebase (B) / before reading the documentation. (C) / No error (D)'

- **A**: Part A
- **B**: Part B
- **C**: Part C
- **D**: Part D

**Correct Answer**: **A**

#### Why
'Hardly' is already negative. Pairing 'hardly' with 'nothing' creates an ungrammatical double negative. It must be 'hardly knew anything'.

- **Exam Tip**: 'Hardly', 'barely', and 'scarcely' take 'anything', never 'nothing'.
- **Source**: Added practice

---
### COM-027: Sentence Rearrangement: Incident Post-Mortem

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Parajumbles

#### Prompt / Question
Arrange the sentences in logical sequence:
1. Root-cause analysis traced the memory leak to unclosed database connections.
2. Alerts fired indicating elevated server latency.
3. A hotfix was deployed to wrap connection pools in try-with-resources blocks.
4. Site reliability engineers stabilized the node by restarting the service.

- **A**: 2 - 4 - 1 - 3
- **B**: 1 - 2 - 3 - 4
- **C**: 2 - 1 - 4 - 3
- **D**: 4 - 2 - 1 - 3

**Correct Answer**: **A**

#### Why
Incident sequence: Alert fires (2) -> Immediate service restart to stabilize (4) -> In-depth root-cause investigation (1) -> Permanent hotfix release (3).

- **Exam Tip**: Order: Detection -> Containment -> Root Cause -> Remediation.
- **Source**: Added practice

---
### COM-028: Grammar: Correct Use of 'Fewer' vs 'Less'

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Grammar Mechanics

#### Prompt / Question
Fill in the blanks:
'The refactored module contains ________ lines of code and consumes ________ memory.'

- **A**: fewer, less
- **B**: less, fewer
- **C**: fewer, fewer
- **D**: less, less

**Correct Answer**: **A**

#### Why
'Fewer' is used for countable nouns ('lines of code'). 'Less' is used for uncountable quantities ('memory').

- **Exam Tip**: Countable = Fewer; Uncountable = Less.
- **Source**: Added practice

---
### COM-029: Vocabulary: Formal Corporate Idioms

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Vocabulary

#### Prompt / Question
In corporate project governance, what does the idiom **'TOUCH BASE'** mean?
'Let us touch base on Thursday before submitting the deliverables.'

- **A**: Cancel the deliverables
- **B**: Briefly make contact or check in to align progress
- **C**: Sign a legal contract
- **D**: Conduct an in-depth audit

**Correct Answer**: **B**

#### Why
'Touch base' means to briefly meet, call, or email someone to confirm plans or update each other.

- **Exam Tip**: Touch base = Briefly connect / Check in.
- **Source**: Added practice

---
### COM-030: Extempore Prompt: Work-From-Home vs In-Office Speaking Model

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Spoken English

#### Prompt / Question
When responding to the prompt 'Remote Work vs In-Office Collaboration', which opening statement is most effective for speech clarity?

- **A**: "Uh, well, I personally think remote work is cool because I sleep more."
- **B**: "The debate between remote and in-office work centers on balancing individual productivity with collaborative synergy."
- **C**: "Nobody likes traveling to the office in the morning traffic."
- **D**: "There are pros and cons to both sides."

**Correct Answer**: **B**

#### Why
Option B introduces the core conceptual dichotomy with elevated professional vocabulary ('productivity', 'collaborative synergy'), providing an instant high score on vocabulary and structural competence.

- **Exam Tip**: Open extempore with a balanced, thesis-driven framing statement.
- **Source**: Added practice

---


---

Previous: [README.md](README.md) | Next: [../08-recent-patterns/README.md](../08-recent-patterns/README.md)
