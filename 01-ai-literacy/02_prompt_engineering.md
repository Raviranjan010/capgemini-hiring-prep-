[Home](../README.md) > [01-ai-literacy](README.md) > 02_prompt_engineering.md

# 02. Prompt Engineering Frameworks & Techniques

## Learn

### 1. The "RTC-FC" Prompt Structure
To eliminate ambiguity and prevent hallucinations, structure enterprise prompts using **RTC-FC**:
1. **R - Role**: Who the AI acts as (e.g. *"Senior Java Architect"*).
2. **T - Task**: The specific objective (e.g. *"Refactor this loop into a stream pipeline"*).
3. **C - Context**: Essential background information, API signatures, or environment details.
4. **F - Format**: Expected output schema (e.g. *"Valid JSON adhering to the provided schema"*).
5. **C - Constraints**: Boundaries, rules, and negative instructions (e.g. *"Do not import external libraries"*).

- **Real-Life Analogy**: Giving instructions to a summer intern. If you say "write an email," you get random results. If you specify role, purpose, audience, and word count, you get exactly what you need.
- **Exam Angle**: Capgemini asks candidates to identify which prompt component is missing or which framework resolves ambiguous responses.

### 2. Prompting Techniques Taxonomy
- **Zero-Shot**: Asking the model to solve a problem with no prior examples.
- **Few-Shot**: Providing 2 to 5 high-quality input-output demonstration pairs inside the prompt before asking the target question.
- **Chain-of-Thought (CoT)**: Prompting the model to output intermediate reasoning steps before the final answer (*"Let's think step by step"*).
- **Self-Consistency**: Generating multiple reasoning paths at higher temperature and taking the majority vote answer.
- **Prompt Chaining**: Decomposing a complex multi-step workflow into a sequence of smaller, focused single-task prompts where the output of prompt $N$ becomes input to prompt $N+1$.

---

## Practice
### AI-003: Intermediate Reasoning via Chain-of-Thought

**Tag**: [VIDEO] | **Difficulty**: Easy | **Topic**: Prompt Engineering

#### Question
When evaluating complex mathematical or algorithmic logic, what prompting technique significantly reduces reasoning errors by forcing intermediate deductions?

- **A**: Zero-shot direct answer generation
- **B**: Chain-of-Thought (CoT) prompting ('Let us think step by step')
- **C**: Temperature max-sampling
- **D**: Vocabulary truncation

**Correct Answer**: **B**

#### Why
Chain-of-Thought prompting instructs the model to explicitly decompose a multi-step problem into sequential deductions before outputting the final token. This expands computational reasoning across multiple autoregressive passes.

- **5-Second Shortcut**: Chain-of-Thought = step-by-step reasoning tokens before final answer.
- **Trap**: Thinking zero-shot direct generation is more accurate for math. Models fail if forced to output answer tokens immediately.
- **Source**: KN Academy Video: https://youtu.be/o5TbT3kzEnA

---

### AI-012: Few-Shot vs. Zero-Shot Prompting

**Tag**: [ADDED] | **Difficulty**: Easy | **Topic**: Prompt Paradigms

#### Question
Which of the following scenarios best demonstrates few-shot prompting?

- **A**: Asking an LLM to translate a sentence with no examples provided
- **B**: Supplying three labeled pairs of customer feedback and sentiment classifications before providing the target sentence
- **C**: Fine-tuning model weights using gradient descent on 10,000 documents
- **D**: Setting temperature to zero

**Correct Answer**: **B**

#### Why
Few-shot prompting conditions the model by embedding 2 to 5 demonstration examples inside the prompt context, establishing formatting and stylistic priors without modifying model parameters.

- **5-Second Shortcut**: Few-shot = prompt contains examples (no weight updates).
- **Trap**: Confusing few-shot prompting with fine-tuning. Few-shot lives entirely in the prompt context.
- **Source**: Capgemini Candidate Exam Debriefs

---

### AI-014: Anatomy of an Effective Prompt

**Tag**: [ADDED] | **Difficulty**: Easy | **Topic**: RTC-FC Framework

#### Question
Which of the following is NOT an essential component of the enterprise RTC-FC prompt framework?

- **A**: Role definition
- **B**: Specific task and format constraints
- **C**: Real-time GPU thread allocation command
- **D**: Contextual domain parameters

**Correct Answer**: **C**

#### Why
The RTC-FC framework governs prompt engineering: Role, Task, Context, Format, and Constraints. GPU thread allocation is an infrastructure configuration managed by the inference runtime, completely separate from natural language prompts.

- **5-Second Shortcut**: RTC-FC = Role, Task, Context, Format, Constraints.
- **Trap**: Assuming prompt text controls physical GPU hardware directly.
- **Source**: Capgemini Candidate Exam Debriefs

---

### AI-022: Multi-Step Math Failure & Reasoning Chains

**Tag**: [CHAT] | **Difficulty**: Medium | **Topic**: Complex Reasoning

#### Question
An LLM fails on a 5-step inventory depreciation calculation when asked for a direct answer. Which prompting strategy best resolves this failure?

- **A**: Increasing temperature to 1.8
- **B**: Enforcing Chain-of-Thought reasoning by instructing the model to show all intermediate ledger balances before the final result
- **C**: Removing all punctuation from the prompt
- **D**: Switching to an unaligned base model

**Correct Answer**: **B**

#### Why
Direct generation forces the model to predict the final number token in a single forward pass without 'thinking time'. CoT forces generation of intermediate tokens, allowing subsequent predictions to attend to previous calculations.

- **5-Second Shortcut**: Math failure = apply Chain-of-Thought (CoT).
- **Trap**: Increasing temperature to fix logic errors. High temperature increases variance and makes math worse.
- **Source**: Capgemini Candidate Exam Debriefs

---

### AI-026: Self-Consistency Decoding Strategy

**Tag**: [CHAT] | **Difficulty**: Medium | **Topic**: Advanced Prompting

#### Question
How does the 'Self-Consistency' prompting strategy improve reasoning reliability over standard Greedy Chain-of-Thought?

- **A**: It re-trains the model on Wikipedia
- **B**: It samples multiple diverse reasoning paths at non-zero temperature and selects the final answer that appears most frequently (majority voting)
- **C**: It rejects any answer containing numbers
- **D**: It strictly uses temperature 0.0 across 10 iterations

**Correct Answer**: **B**

#### Why
Self-Consistency samples multiple parallel CoT reasoning trajectories (e.g. 5-10 paths with $T=0.7$) and performs majority voting over the final answers. Random reasoning errors average out across multiple paths.

- **5-Second Shortcut**: Self-consistency = multi-path sampling + majority vote.
- **Trap**: Running 10 paths at temperature 0.0. Temperature 0 is deterministic, producing identical duplicate paths.
- **Source**: Capgemini Candidate Exam Debriefs

---

### AI-027: Prompt Chaining vs. Single Long Prompt

**Tag**: [CHAT] | **Difficulty**: Medium | **Topic**: Pipeline Architecture

#### Question
Why is prompt chaining (breaking tasks into sequential sub-prompts) preferred over a single complex 3,000-word prompt for automated legal contract review?

- **A**: Prompt chaining reduces total API calls to exactly one
- **B**: Single mega-prompts suffer from instruction dilution and partial compliance, while prompt chaining isolates validation and reduces error propagation
- **C**: Prompt chaining eliminates the need for an LLM API
- **D**: Single long prompts always cost 10x more per token

**Correct Answer**: **B**

#### Why
When an LLM is given 15 simultaneous complex instructions in a single prompt, attention dilution leads to missed requirements. Chaining isolates each step (extract $\to$ validate $\to$ summarize), enabling validation between stages.

- **5-Second Shortcut**: Divide and conquer: modular chained prompts beat one massive prompt.
- **Trap**: Assuming a single mega-prompt is more reliable because the model 'sees everything at once'. Long prompts lead to skipped constraints.
- **Source**: Capgemini Candidate Exam Debriefs

---

### AI-045: Zero-Shot Chain-of-Thought (CoT) Prompting

**Tag**: [VIDEO] | **Difficulty**: Easy | **Topic**: Prompt Engineering

#### Question
Which famous 5-word phrase triggers zero-shot Chain-of-Thought reasoning in instruction-tuned LLMs?

- **A**: Answer this immediately without hesitation
- **B**: "Let's think step by step"
- **C**: Generate in valid JSON format
- **D**: Translate into professional corporate English

**Correct Answer**: **B**

#### Why
Kojima et al. demonstrated that appending the trigger phrase "Let's think step by step" to a prompt induces the model to generate an intermediate rationale, drastically increasing accuracy on zero-shot reasoning benchmarks.

- **5-Second Shortcut**: "Let's think step by step" = zero-shot CoT trigger.
- **Trap**: Assuming CoT always requires writing complex few-shot examples. A simple 5-word suffix enables zero-shot CoT.
- **Source**: KN Academy Video: https://youtu.be/o5TbT3kzEnA

---

### AI-066: Role Prompting Impact on Output Quality

**Tag**: [ADDED] | **Difficulty**: Easy | **Topic**: Role Definition

#### Question
In the RTC-FC framework, what is the primary operational effect of prepending 'Act as an expert cybersecurity auditor' to a vulnerability assessment prompt?

- **A**: It unlocks restricted administrative root permissions on the server
- **B**: It steers the attention mechanism toward specialized vocabulary, technical depth, and security-oriented latent representations learned during training
- **C**: It speeds up inference throughput by 50%
- **D**: It guarantees 100% detection of zero-day exploits

**Correct Answer**: **B**

#### Why
Role prompting acts as a semantic conditioning prior, weighting domain-specific latent representations and vocabulary so the model responds with appropriate technical rigor rather than generic lay explanations.

- **5-Second Shortcut**: Role prompting = conditions latent space for domain-specific terminology and depth.
- **Trap**: Believing role prompts grant actual operating system privileges or security clearances.
- **Source**: Added practice: Prompt framing fundamentals

---

### AI-067: Negative Constraints in Prompting

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Constraint Engineering

#### Question
When designing prompt constraints, why is 'Do NOT use recursion; use an iterative while-loop' sometimes less reliable than 'Implement using strictly an iterative while-loop'?

- **A**: LLMs cannot read negative words like 'NOT'
- **B**: LLMs predict tokens based on positive semantic associations; mentioning 'recursion' primes the attention weights with recursive patterns, increasing error probability under weak attention
- **C**: Negative words automatically trigger safety filters
- **D**: Iterative algorithms always require more memory

**Correct Answer**: **B**

#### Why
LLMs process tokens via positive association. When a prompt says 'Do NOT mention pricing', the word 'pricing' is heavily activated. Positive framing ('Focus exclusively on technical specs') provides cleaner directional steering.

- **5-Second Shortcut**: Frame constraints positively: specify what to do rather than what to avoid.
- **Trap**: Repeating prohibited words 10 times in a prompt. This actually primes the model to output them.
- **Source**: Pattern practice: Negative constraint avoidance

---

### AI-068: Least-to-Most Prompting

**Tag**: [ADDED] | **Difficulty**: Medium | **Topic**: Advanced Reasoning

#### Question
How does 'Least-to-Most' prompting differ from standard Chain-of-Thought prompting?

- **A**: It sorts training documents by file size
- **B**: It explicitly decomposes a problem into a series of simpler subproblems, solves the easiest subproblem first, and passes the result into the next harder subproblem
- **C**: It uses the smallest available model to verify answers
- **D**: It restricts vocabulary to the 1,000 most common words

**Correct Answer**: **B**

#### Why
Least-to-most prompting decomposes complex multi-layered tasks into a dependency graph of sub-questions, sequentially solving foundational steps before tackling the composite goal.

- **5-Second Shortcut**: Least-to-Most = decompose into subproblems, solve from simplest to hardest.
- **Trap**: Thinking it refers to sorting tokens by probability. It is an algorithmic problem decomposition technique.
- **Source**: Added practice: Multi-step reasoning paradigms

---

### AI-069: Directional Stimulus Prompting

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Prompt Guidance

#### Question
What is 'Directional Stimulus Prompting' in automated text generation?

- **A**: Pointing a webcam at the user during typing
- **B**: Providing a small hint or set of trigger keywords to guide the generation trajectory toward a specific aspect without writing the full text
- **C**: Hardcoding the temperature parameter to exactly 0.5
- **D**: Running model inference in reverse

**Correct Answer**: **B**

#### Why
Directional stimulus prompting inserts short guidance keywords (e.g. 'Key points: latency, caching, horizontal scaling') into the prompt to steer the generated narrative toward specific themes.

- **5-Second Shortcut**: Directional stimulus = hint keywords guiding narrative focus.
- **Trap**: Confusing with few-shot examples. Hints are brief trigger keywords, not full input-output pairs.
- **Source**: Pattern practice: Trajectory steering

---

### AI-070: Format Enforcement via Delimiters

**Tag**: [ADDED] | **Difficulty**: Easy | **Topic**: Delimiter Engineering

#### Question
Why is wrapping user inputs in clear delimiters (such as `"""` or `<user_data>...</user_data>`) considered best practice in prompt design?

- **A**: It automatically compresses the text by 50%
- **B**: It clearly demarcates instruction boundaries from data payloads, preventing instruction confusion and accidental prompt injection
- **C**: It compiles the prompt into Markdown AST format
- **D**: It forces the GPU to allocate shared memory

**Correct Answer**: **B**

#### Why
Delimiters create explicit structural boundaries between system commands and variable user input, helping the attention mechanism distinguish instructions from data.

- **5-Second Shortcut**: Delimiters separate instructions from user data.
- **Trap**: Believing delimiters are just decorative styling. They are critical for structural parsing and security.
- **Source**: Added practice: Delimiter engineering

---

### AI-071: Rephrase and Respond (RaR) Technique

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Prompt Refinement

#### Question
What is the operational premise of the 'Rephrase and Respond' (RaR) prompting technique?

- **A**: Translating the prompt into French and back into English
- **B**: Instructing the LLM to first rephrase and expand the user's ambiguous query to clarify intent before generating the final answer
- **C**: Rejecting any query that has more than 20 words
- **D**: Replacing technical terms with dictionary definitions

**Correct Answer**: **B**

#### Why
RaR instructs the model to rewrite the input question into a clear, unambiguous reformulation first. This ensures the model aligns on the exact problem requirements before generating the solution.

- **5-Second Shortcut**: Rephrase and Respond = model clarifies question before answering.
- **Trap**: Assuming the user must always write perfect prompts. The model can self-disambiguate via RaR.
- **Source**: Pattern practice: Query clarification paradigms

---

### AI-072: Contrastive Prompting

**Tag**: [ADDED] | **Difficulty**: Medium | **Topic**: Few-Shot Engineering

#### Question
What is 'Contrastive Prompting' in few-shot demonstrations?

- **A**: Providing only incorrect examples to show what not to do
- **B**: Providing paired examples showing both the correct solution AND a common incorrect attempt with an explanation of why it failed
- **C**: Using high-contrast black and white text colors in the prompt UI
- **D**: Comparing outputs of two different LLM providers

**Correct Answer**: **B**

#### Why
Contrastive prompting provides both positive and negative exemplars side-by-side, explicitly teaching the model the boundary between valid reasoning and common candidate traps.

- **5-Second Shortcut**: Contrastive prompting = shows correct vs incorrect example pairs.
- **Trap**: Thinking contrastive prompting only shows negative examples. It must show both for comparison.
- **Source**: Added practice: Demonstration design

---

### AI-073: Token Economy in Prompt Engineering

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: Token Efficiency

#### Question
In a timed coding assessment with a strict 1,000-token budget, which prompt strategy is most token-efficient?

- **A**: Pasting the entire problem statement and asking 10 general questions at once
- **B**: Sending a concise RTC prompt with exact method signatures, minimal constraints, and requesting only code with zero conversational pleasantries
- **C**: Engaging in casual multi-turn greeting dialogue to build rapport with the AI
- **D**: Using few-shot examples with 500 lines of sample input

**Correct Answer**: **B**

#### Why
Conversational filler ('Hello', 'Thank you', verbose explanations) rapidly consumes token budgets. High-scoring candidates use terse, structured prompts specifying input/output types and constraints directly.

- **5-Second Shortcut**: Cut pleasantries; specify types, constraints, and request raw code.
- **Trap**: Adding polite conversational chatter. AI models don't need pleasantries, and tokens cost score.
- **Source**: Pattern practice: Assessment token conservation

---

### AI-074: Prompt Drift Across Extended Conversations

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Attention Drift

#### Question
During a 15-turn coding interaction, the model begins ignoring a constraint established in Turn 1. What technique mitigates this 'prompt drift'?

- **A**: Restarting the computer
- **B**: Re-injecting or summarizing critical constraints in the latest prompt or using structured system prompt reminders
- **C**: Increasing the temperature to 2.0
- **D**: Removing all indentation from code snippets

**Correct Answer**: **B**

#### Why
As context grows, older tokens experience attention decay. Explicitly reminding the model of key invariants in subsequent turns or using system-level instruction anchoring maintains compliance.

- **5-Second Shortcut**: Re-inject critical constraints in subsequent turns to prevent drift.
- **Trap**: Assuming models remember Turn 1 with 100% fidelity after 15 long code turns. Attention decays over distance.
- **Source**: Pattern practice: Multi-turn session management

---

### AI-075: Chain-of-Thought in Code Generation

**Tag**: [ADDED] | **Difficulty**: Medium | **Topic**: Algorithmic Prompting

#### Question
When asking an LLM to generate code for a complex graph problem, why should you instruct it to output algorithmic steps *before* the code block?

- **A**: Compilers require English comments at the top of the file
- **B**: Generating the textual algorithmic steps first forces the model to attend to those valid reasoning tokens when predicting the subsequent code tokens
- **C**: It doubles the generation speed of the GPU
- **D**: It bypasses copyright detection filters

**Correct Answer**: **B**

#### Why
Because LLMs are autoregressive, the tokens generated earlier become the context for later tokens. Outputting logic steps first primes the attention heads with valid algorithmic invariants before writing code syntax.

- **5-Second Shortcut**: Explain logic first $\to$ code tokens attend to valid logic context.
- **Trap**: Asking for code first and explanation after. Code written before logic is prone to ungrounded bugs.
- **Source**: Added practice: Code generation prompting

---

## Sources for This File
- KN Academy Video: [Capgemini Complete Recruitment Pattern](https://youtu.be/o5TbT3kzEnA)
- KN Academy Playlist: [Capgemini 2026/2027 Masterclass](https://www.youtube.com/watch?v=mLaYLknw4KU&list=PLGFjgYQtw1UjDBEkL2Edqej2kXbxGA1NO)
- Gemini candidate chat archives in `Capgemini Candidate Exam Debriefs`

---

Previous: [01_llm_basics.md](01_llm_basics.md) | Next: [03_rag_and_vectors.md](03_rag_and_vectors.md)
