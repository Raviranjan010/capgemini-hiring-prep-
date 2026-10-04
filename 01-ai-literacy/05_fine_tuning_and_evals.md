[Home](../README.md) > [01-ai-literacy](README.md) > 05_fine_tuning_and_evals.md

# 05. Fine-Tuning, Parameter-Efficient Adaptation (LoRA) & Evaluation Metrics

## Learn

### 1. Fine-Tuning vs Prompting vs RAG
| Dimension | Prompt Engineering | RAG (Retrieval) | Fine-Tuning (SFT) |
| :--- | :--- | :--- | :--- |
| **Model Weights** | Frozen (Unchanged) | Frozen (Unchanged) | **Updated via Backprop** |
| **New Knowledge** | Zero (In context only) | **Dynamic external docs** | Fixed to training snapshot |
| **Best Used For** | Rapid prototyping, zero setup | Dynamic enterprise facts, search | Specialized style, syntax, tone |
| **Cost & Latency** | Low cost, context-bound | Modest retrieval latency | High GPU training compute |

- **Real-Life Analogy**: Prompting is giving an employee an instruction manual. RAG is giving them internet search access during work. Fine-tuning is sending them to a 6-month specialized graduate school.
- **Exam Angle**: Capgemini asks candidates to choose between RAG and fine-tuning for rapidly changing data vs domain-specific formatting.

### 2. Parameter-Efficient Fine-Tuning (PEFT) & LoRA
- Full fine-tuning updates all billions of model weights, requiring massive VRAM and storage.
- **LoRA (Low-Rank Adaptation)**: Freezes original weights $W_0 \in \mathbb{R}^{d \times k}$ and injects trainable rank decomposition matrices:
  $$W = W_0 + \Delta W = W_0 + B \cdot A$$
  where $B \in \mathbb{R}^{d \times r}$ and $A \in \mathbb{R}^{r \times k}$ with rank $r \ll \min(d, k)$ (e.g. $r=8$ or $16$).
- **Advantage**: Reduces trainable parameters by over 99% (e.g. training only 10MB of weights instead of a 28GB model).

### 3. RLHF (Reinforcement Learning from Human Feedback)
1. **SFT (Supervised Fine-Tuning)**: Train base model on high-quality human demonstration Q&A pairs.
2. **Reward Model Training**: Human evaluators rank multiple model completions ($A > B > C$). A reward model learns to score answers.
3. **PPO / DPO Optimization**: The policy is optimized to maximize reward while penalizing divergence from the base model via KL-divergence penalty.

### 4. RAG Evaluation Metrics (The RAGAS Triad)
- **Faithfulness**: Measures whether the generated answer is grounded *strictly* in the retrieved context chunks (penalizes hallucinations).
- **Answer Relevance**: Measures whether the generated response directly addresses the user's input query.
- **Context Recall**: Measures whether the retriever fetched *all* relevant factual chunks needed to answer the question.

---

## Practice
### AI-016: Fine-Tuning vs. Prompt Engineering

**Tag**: [ADDED] | **Difficulty**: Easy | **Topic**: Adaptation Paradigms

#### Question
Which statement accurately distinguishes fine-tuning from prompt engineering?

- **A**: Prompt engineering permanently alters neural network weights via backpropagation
- **B**: Fine-tuning modifies model weights via supervised gradient descent; prompt engineering modifies only runtime context inputs without weight changes
- **C**: Fine-tuning requires zero training data
- **D**: Prompt engineering can only run on CPU servers

**Correct Answer**: **B**

#### Why
Prompt engineering conditions the model at inference time without modifying internal parameters. Fine-tuning executes gradient descent passes across a curated dataset, permanently updating numerical layer weights.

- **5-Second Shortcut**: Fine-tuning = modifies weights; Prompting = modifies context input.
- **Trap**: Thinking prompt engineering updates model memory permanently. When the API call finishes, context vanishes.
- **Source**: Capgemini Candidate Exam Debriefs

---

### AI-029: RLHF Reward Model Objective

**Tag**: [CHAT] | **Difficulty**: Medium | **Topic**: Alignment Protocols

#### Question
What is the core objective of the Reward Model in a Reinforcement Learning from Human Feedback (RLHF) pipeline?

- **A**: To compress model weights onto mobile devices
- **B**: To score candidate completions by outputting a scalar value reflecting human preferences for helpfulness, accuracy, and safety
- **C**: To translate text between English and Spanish
- **D**: To monitor GPU temperatures

**Correct Answer**: **B**

#### Why
The Reward Model is trained on pairwise human rankings (e.g. Response A is preferred over Response B). It outputs a scalar numerical score used by reinforcement algorithms (PPO) to steer the policy model toward helpful, harmless responses.

- **5-Second Shortcut**: Reward model = predicts human preference score (scalar reward).
- **Trap**: Confusing the reward model with the generator. The reward model acts as a judge scoring completions.
- **Source**: Capgemini Candidate Exam Debriefs

---

### AI-035: Hallucination Evaluation Frameworks (Faithfulness vs. Overlap)

**Tag**: [CHAT] | **Difficulty**: Medium | **Topic**: Evaluation Metrics

#### Question
Why is traditional n-gram overlap (such as ROUGE or BLEU) insufficient for evaluating factual accuracy in RAG systems, necessitating metrics like RAGAS Faithfulness?

- **A**: BLEU scores only work on images
- **B**: ROUGE and BLEU measure superficial lexical word overlap without understanding semantic truth; a response can have high lexical overlap while completely reversing factual polarity
- **C**: RAGAS requires no computation
- **D**: BLEU is an open-source model

**Correct Answer**: **B**

#### Why
ROUGE measures word matching. For example, 'The patient does NOT have cancer' shares 85% ROUGE overlap with 'The patient DOES have cancer', despite complete medical contradiction. Faithfulness checks entailment and factual groundedness.

- **5-Second Shortcut**: ROUGE/BLEU = lexical word overlap; Faithfulness = semantic factual truth.
- **Trap**: Assuming high BLEU score proves an answer is factually correct. Surface word overlap ignores negation.
- **Source**: Capgemini Candidate Exam Debriefs

---

### AI-051: RAGAS Metrics: Context Recall vs. Faithfulness

**Tag**: [VIDEO] | **Difficulty**: Medium | **Topic**: Evaluation Frameworks

#### Question
An automated RAG pipeline retrieves 3 irrelevant documents. The LLM ignores them and uses its pre-trained parametric memory to generate a correct real-world answer. How will RAGAS evaluate this output?

- **A**: High Faithfulness, High Context Recall
- **B**: Low Faithfulness (not grounded in context), High Answer Relevance
- **C**: Zero Answer Relevance
- **D**: High Context Recall, Low Answer Relevance

**Correct Answer**: **B**

#### Why
In RAGAS, Faithfulness measures whether claims are derived *strictly* from retrieved context. Even if the answer is factually true in real life, because it was not grounded in the provided documents, Faithfulness is scored near zero.

- **5-Second Shortcut**: Faithfulness = grounded in retrieved docs (not real-world facts from memory).
- **Trap**: Believing real-world truth equals RAG faithfulness. If it isn't in the retrieved text, it fails faithfulness.
- **Source**: KN Academy Video: https://youtu.be/o5TbT3kzEnA

---

### AI-053: LoRA Parameter-Efficient Fine-Tuning

**Tag**: [VIDEO] | **Difficulty**: Hard | **Topic**: Model Adaptation

#### Question
In LoRA (Low-Rank Adaptation), how does decomposing weight updates into two low-rank matrices $B \cdot A$ (where $A \in \mathbb{R}^{r \times k}$ and $B \in \mathbb{R}^{d \times r}$) reduce memory and parameter count?

- **A**: It sets all original weights to zero
- **B**: Choosing rank $r \ll \min(d, k)$ reduces the parameter count from $d \times k$ to $r \times (d + k)$, allowing fine-tuning with a fraction of the trainable weights
- **C**: It quantizes all floats to 1 bit
- **D**: It eliminates the backpropagation step entirely

**Correct Answer**: **B**

#### Why
If $d=4096, k=4096$, full matrix update requires $4096 \times 4096 \approx 16.7\text{M}$ parameters. With LoRA rank $r=8$, trainable parameters are $8 \times (4096 + 4096) = 65,536$ parameters—a 99.6% reduction.

- **5-Second Shortcut**: LoRA rank $r$: trains $r \times (d+k)$ parameters instead of $d \times k$.
- **Trap**: Assuming LoRA modifies the original weight matrix directly during training. Original weights stay frozen.
- **Source**: KN Academy Video: https://youtu.be/o5TbT3kzEnA

---

### AI-097: Catastrophic Forgetting in Full Fine-Tuning

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Fine-Tuning Risks

#### Question
What is 'Catastrophic Forgetting' when fine-tuning a general-purpose foundation model on a narrow domain dataset?

- **A**: The server hard drive crashes during training
- **B**: The model's weights overfit to the new domain, causing severe degradation or complete loss of its general reasoning and broad language capabilities learned during pre-training
- **C**: The tokenizer loses half its vocabulary
- **D**: The context window decreases to 512 tokens

**Correct Answer**: **B**

#### Why
When weights are updated aggressively on a single specialized task (e.g. legal text), the model's generalized abilities across math, coding, and common sense degrade drastically. PEFT/LoRA helps mitigate catastrophic forgetting.

- **5-Second Shortcut**: Catastrophic forgetting = specialized fine-tuning erases general reasoning.
- **Trap**: Assuming fine-tuning strictly adds new knowledge without affecting existing capabilities.
- **Source**: Pattern practice: Fine-tuning failure modes

---

### AI-098: DPO (Direct Preference Optimization) vs PPO

**Tag**: [PATTERN] | **Difficulty**: Hard | **Topic**: Alignment Algorithms

#### Question
What is the primary architectural simplification of Direct Preference Optimization (DPO) compared to traditional PPO-based RLHF?

- **A**: DPO eliminates the need for human preference data
- **B**: DPO mathematically derives policy updates directly from preferred/dispreferred data without requiring training an explicit separate Reward Model or running reinforcement learning loops
- **C**: DPO runs exclusively on edge devices
- **D**: DPO works without GPU hardware

**Correct Answer**: **B**

#### Why
Rafailov et al. proved that the reward model can be mathematically substituted directly into the loss function, allowing direct optimization on preferred vs dispreferred pairs ($y_w > y_l$) via standard cross-entropy, eliminating complex PPO actor-critic stabilization.

- **5-Second Shortcut**: DPO = direct preference optimization without training a separate reward model.
- **Trap**: Thinking DPO requires training 4 separate models like PPO. DPO uses standard supervised cross-entropy loss.
- **Source**: Pattern practice: Modern alignment pipelines

---

### AI-099: Context Precision Metric

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: RAGAS Metrics

#### Question
In the RAGAS framework, what does 'Context Precision' evaluate?

- **A**: The number of tokens in the prompt
- **B**: Whether the highest-relevance context chunks are positioned at the very top of the retrieved ranking list
- **C**: The GPU memory precision (FP16 vs INT8)
- **D**: The reading speed of the user

**Correct Answer**: **B**

#### Why
Context Precision penalizes systems where relevant information is buried at lower ranks. High context precision means all signal-bearing chunks are positioned at the top of the context, mitigating lost-in-the-middle degradation.

- **5-Second Shortcut**: Context Precision = relevant chunks placed at top ranks of context.
- **Trap**: Confusing Context Precision with Context Recall. Precision is rank placement; Recall is finding all facts.
- **Source**: Pattern practice: RAG evaluation metrics

---

### AI-100: KL-Divergence Penalty in RLHF

**Tag**: [ADDED] | **Difficulty**: Hard | **Topic**: RLHF Mechanics

#### Question
Why is a Kullback-Leibler (KL) divergence penalty incorporated into the RLHF objective function: $\max_\theta \mathbb{E}[R(x, y)] - \beta D_{KL}(\pi_\theta \parallel \pi_{ref})$?

- **A**: To maximize training loss
- **B**: To prevent the fine-tuned policy model $\pi_\theta$ from drifting too far from the original reference model $\pi_{ref}$, preventing 'reward hacking' (generating unreadable gibberish that tricks the reward model)
- **C**: To compress model weights
- **D**: To speed up gradient descent

**Correct Answer**: **B**

#### Why
Without the KL penalty, reinforcement learning models 'hack' the reward model by emitting repetitive reward-triggering buzzwords. The KL penalty enforces proximity to the original coherent base model.

- **5-Second Shortcut**: KL penalty prevents reward hacking and maintains linguistic coherence.
- **Trap**: Assuming reinforcement learning can run without bounds. Models drift into nonsense without KL regularization.
- **Source**: Added practice: RLHF loss functions

---

### AI-101: QLoRA (Quantized Low-Rank Adaptation)

**Tag**: [PATTERN] | **Difficulty**: Hard | **Topic**: PEFT Innovations

#### Question
What core innovation does QLoRA introduce on top of standard LoRA?

- **A**: It converts all LoRA matrices to 1-bit integers
- **B**: It quantizes the frozen base model weights to 4-bit NormalFloat (NF4) while maintaining 16-bit LoRA adapter weights, allowing 65B models to be fine-tuned on a single 48GB GPU
- **C**: It removes the attention mechanism
- **D**: It replaces Transformers with RNNs

**Correct Answer**: **B**

#### Why
Dettmers et al. introduced QLoRA, combining 4-bit NormalFloat quantization of frozen base weights with Double Quantization and Paged Optimizers, drastically reducing VRAM while maintaining full 16-bit fine-tuning accuracy.

- **5-Second Shortcut**: QLoRA = 4-bit quantized base model + 16-bit LoRA adapters.
- **Trap**: Thinking QLoRA quantizes the adapter matrices to 4-bit. Adapters remain in 16-bit for accurate gradient backprop.
- **Source**: Pattern practice: Resource-efficient fine-tuning

---

### AI-102: Instruction Tuning Dataset Formatting (Alpaca / ShareGPT)

**Tag**: [ADDED] | **Difficulty**: Easy | **Topic**: Data Preparation

#### Question
In supervised instruction fine-tuning, what standard structure does an Alpaca-style training sample follow?

- **A**: A single continuous string of Wikipedia text with no labels
- **B**: Instruction, optional Input context, and desired Output completion
- **C**: A compiled Python AST object
- **D**: A SQL table schema

**Correct Answer**: **B**

#### Why
Instruction datasets format training examples into three distinct semantic keys: `instruction` (the command), `input` (supporting text/data), and `output` (the target high-quality response).

- **5-Second Shortcut**: Instruction dataset structure = Instruction + Input + Output.
- **Trap**: Using raw unstructured text for instruction tuning. Unstructured text is for pre-training; SFT requires labeled pairs.
- **Source**: Added practice: Training dataset architecture

---

### AI-103: Perplexity (PPL) Metric

**Tag**: [ADDED] | **Difficulty**: Medium | **Topic**: Language Model Metrics

#### Question
What does a lower Perplexity score ($PPL = \exp(Loss)$) indicate about a language model evaluated on a test text corpus?

- **A**: The model has high hallucination rate
- **B**: The model is less surprised by the test text and assigns higher probability to the ground-truth tokens
- **C**: The model is running at higher GPU temperature
- **D**: The model has fewer parameters

**Correct Answer**: **B**

#### Why
Perplexity is the exponentiated cross-entropy loss. Lower perplexity indicates that the probability distribution predicted by the model aligns more closely with the actual test token distribution (higher prediction certainty).

- **5-Second Shortcut**: Lower perplexity = model is less surprised (better language prediction).
- **Trap**: Thinking high perplexity is good. Lower perplexity indicates superior probabilistic prediction.
- **Source**: Added practice: Evaluation metrics

---

### AI-104: ROUGE-L Metric Mechanics

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Summarization Evaluation

#### Question
What specific grammatical property does the ROUGE-L metric measure in text summarization evaluation?

- **A**: Length of the longest word in the summary
- **B**: Longest Common Subsequence (LCS) between the candidate summary and the reference text, capturing sentence-level structure without requiring consecutive matches
- **C**: The sentiment polarity of adjectives
- **D**: The number of punctuation marks

**Correct Answer**: **B**

#### Why
ROUGE-L calculates the Longest Common Subsequence between generated and reference text. Unlike ROUGE-1/2 which require strict consecutive n-grams, LCS accounts for word order while allowing intervening words.

- **5-Second Shortcut**: ROUGE-L = Longest Common Subsequence (order-aware matching).
- **Trap**: Confusing ROUGE-L with word count length. 'L' stands for Longest Common Subsequence.
- **Source**: Pattern practice: Traditional NLP evaluation

---

### AI-105: LLM-as-a-Judge Evaluation Pattern

**Tag**: [PATTERN] | **Difficulty**: Medium | **Topic**: Automated Evaluation

#### Question
What is the primary risk of using an 'LLM-as-a-Judge' (e.g. GPT-4 scoring student responses) to evaluate model outputs?

- **A**: LLMs cannot generate numbers between 1 and 10
- **B**: Position bias (favoring whichever answer is presented first) and self-enhancement bias (favoring outputs generated by its own model family)
- **C**: GPU memory corruption
- **D**: Automatic deletion of test cases

**Correct Answer**: **B**

#### Why
LLM-as-a-judge exhibits systematic biases: position bias (preferring candidate A over B), length bias (preferring longer, verbose answers regardless of substance), and self-preference bias. Pairwise swapping and rubric anchoring mitigate this.

- **5-Second Shortcut**: LLM-as-a-Judge biases: position bias, verbosity bias, and self-preference.
- **Trap**: Assuming LLM judges are completely objective. They systematically favor longer, familiar responses.
- **Source**: Pattern practice: Modern evaluation architectures

---

### AI-106: SFT vs Continued Pre-training

**Tag**: [ADDED] | **Difficulty**: Medium | **Topic**: Training Hierarchy

#### Question
When an enterprise needs a model to master an entirely new specialized language (like ancient Sanskrit or internal COBOL dialects), which approach is required?

- **A**: A 1-shot prompt
- **B**: Continued domain-adaptive pre-training on raw unlabelled text to update vocabulary and core weights, followed by SFT
- **C**: Zero-shot CoT prompting
- **D**: Increasing temperature to 1.5

**Correct Answer**: **B**

#### Why
Prompting and lightweight SFT cannot instill broad new token vocabularies or fundamental language syntax absent from the pre-training distribution. Continued pre-training on unlabelled domain corpus is necessary to learn foundational representations.

- **5-Second Shortcut**: New languages/domains require Continued Pre-training before SFT.
- **Trap**: Attempting to teach a completely new programming language via 3 prompt examples.
- **Source**: Added practice: Training pipeline selection

---

### AI-107: Answer Relevance vs Answer Correctness

**Tag**: [PATTERN] | **Difficulty**: Easy | **Topic**: RAGAS Metrics

#### Question
A user asks: 'What is the capital of France?' The bot responds: 'Paris is a city in Europe where many tourists visit the Eiffel Tower and enjoy French cuisine.' How is this evaluated?

- **A**: Low Answer Relevance, High Correctness
- **B**: High Answer Relevance, Low Answer Correctness
- **C**: High Answer Relevance (directly addresses capital of France with Paris), but with extraneous information
- **D**: Zero Context Recall

**Correct Answer**: **C**

#### Why
Answer relevance measures whether the generated response directly answers the core query. Here, it identifies Paris correctly, though verbosity slightly lowers precision.

- **5-Second Shortcut**: Answer Relevance = directly addresses the specific query intent.
- **Trap**: Thinking an answer must be a single word to be relevant. Contextual explanations remain relevant if focused on the topic.
- **Source**: Pattern practice: Evaluation metric nuances

---

### AI-108: Human-in-the-Loop (HITL) Validation in Production

**Tag**: [ADDED] | **Difficulty**: Easy | **Topic**: Enterprise Governance

#### Question
In enterprise deployment of generative AI models for medical diagnosis or credit scoring, what role does Human-in-the-Loop (HITL) serve?

- **A**: A human types every token generated by the model
- **B**: Human experts review and validate high-stakes or low-confidence model recommendations before final execution, ensuring safety and accountability
- **C**: Humans manually compute the matrix dot products
- **D**: Humans reboot the GPU every hour

**Correct Answer**: **B**

#### Why
For high-stakes decisions (healthcare, legal, credit underwriting), enterprise regulatory standards prohibit fully autonomous agent execution. HITL ensures a qualified human reviews predictions before commitment.

- **5-Second Shortcut**: HITL = human expert reviews high-stakes/low-confidence AI outputs.
- **Trap**: Assuming full autonomy is legally compliant for high-stakes decisions. Regulations mandate human oversight.
- **Source**: Added practice: Enterprise governance standards

---

## Sources for This File
- KN Academy Video: [Capgemini Complete Recruitment Pattern](https://youtu.be/o5TbT3kzEnA)
- KN Academy Playlist: [Capgemini 2026/2027 Masterclass](https://www.youtube.com/watch?v=mLaYLknw4KU&list=PLGFjgYQtw1UjDBEkL2Edqej2kXbxGA1NO)
- Gemini candidate chat archives in `Capgemini Candidate Exam Debriefs`

---

Previous: [04_safety_security_ethics.md](04_safety_security_ethics.md) | Next: [06_agents_and_architecture.md](06_agents_and_architecture.md)
