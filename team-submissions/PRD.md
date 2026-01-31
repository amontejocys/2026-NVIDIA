# Product Requirements Document (PRD)

**Project Name:** LABS-Solv-V1
**Team Name:** Team Rocket
**GitHub Repository:** https://github.com/amontejocys/2026-NVIDIA

---

> **Note to Students:** > The questions and examples provided in the specific sections below are **prompts to guide your thinking**, not a rigid checklist. 
> * **Adaptability:** If a specific question doesn't fit your strategy, you may skip or adapt it.
> * **Depth:** You are encouraged to go beyond these examples. If there are other critical technical details relevant to your specific approach, please include them.
> * **Goal:** The objective is to convince the reader that you have a solid plan, not just to fill in boxes.

---

## 1. Team Roles & Responsibilities [You can DM the judges this information instead of including it in the repository]

| Role | Name | GitHub Handle | Discord Handle
| :--- | :--- | :--- | :--- |
| **Project Lead** (Architect) | Krishnam | krishnamiiti29 | krishnam0474 |
| **GPU Acceleration PIC** (Builder) | Ariel Montejo| amontejocys | amontejo |
| **Quality Assurance PIC** (Verifier) | Prem Santh| premliorate| premliorated |
| **Technical Marketing PIC** (Storyteller) | Charis | Charis-E-Shiny | charis0261|

---

## 2. The Architecture
**Owner:** Project Lead

### Choice of Quantum Algorithm
* **Algorithm:** QAOA (p=1-2)
    
* **Motivation:** 
    * We selected QAOA because it was designed for combinatory optimizations, with a lower p (depth), we can have a very short and stable circuit. It's ideal as generator of bitstrings.
    *  LABS can be expreseed as sum of terms Z_i and Z_j and terms of mayor locality (like G4), QAOA applies an operator with same locality as objective, that's coincidence permits QAOA resolve the case
    * *Better time to seed, lower time to optimization*: This process uses lower optimization cost, selecting a quantum seeder sample, with a shor depth, the process to get the best bitstring becomes easy
    * Works well with noisy computers
   

### Literature Review
* **Reference:** Quantum Approximate Optimization Algorithm (QAOA), Farhi, Goldstone & Gutmann (2014)
* **Relevance:** 
    * Justifies the use of QAOA with low depth circuits

* **Reference:** [Title, Author, Link]
* **Relevance:** [How does this paper support your plan?]
    * *Example:* "Reference: 'QAOA for MaxCut.' Relevance: Although LABS is different from MaxCut, this paper demonstrates how parameter concentration can speed up optimization, which we hope to replicate."

* **Reference:** [Title, Author, Link]
* **Relevance:** [How does this paper support your plan?]
    * *Example:* "Reference: 'QAOA for MaxCut.' Relevance: Although LABS is different from MaxCut, this paper demonstrates how parameter concentration can speed up optimization, which we hope to replicate."

---

## 3. The Acceleration Strategy
**Owner:** GPU Acceleration PIC

### Quantum Acceleration (CUDA-Q)
* **Strategy:** [How will you use the GPU for the quantum part?]
    * *Example:* "After testing with a single L4, we will target the `nvidia-mgpu` backend to distribute the circuit simulation across multiple L4s for large $N$."
 

### Classical Acceleration (MTS)
* **Strategy:** [The classical search has many opportuntities for GPU acceleration. What will you chose to do?]
    * *Example:* "The standard MTS evaluates neighbors one by one. We will use `cupy` to rewrite the energy function to evaluate a batch of 1,000 neighbor flips simultaneously on the GPU."

### Hardware Targets
* **Dev Environment:** [e.g., Qbraid (CPU) for logic, Brev L4 for initial GPU testing]
* **Production Environment:** [e.g., Brev A100-80GB for final N=50 benchmarks]

---

## 4. The Verification Plan
**Owner:** Quality Assurance PIC

### Unit Testing Strategy
* **Framework:** [e.g., `pytest`, `unittest`]
* **AI Hallucination Guardrails:** [How do you know the AI code is right?]
    * *Example:* "We will require AI-generated kernels to pass a 'property test' (Hypothesis library) ensuring outputs are always within theoretical energy bounds before they are integrated."

### Core Correctness Checks
* **Check 1 (Symmetry):** [Describe a specific physics check]
    * *Example:* "LABS sequence $S$ and its negation $-S$ must have identical energies. We will assert `energy(S) == energy(-S)`."
* **Check 2 (Ground Truth):**
    * *Example:* "For $N=3$, the known optimal energy is 1.0. Our test suite will assert that our GPU kernel returns exactly 1.0 for the sequence `[1, 1, -1]`."

---

## 5. Execution Strategy & Success Metrics
**Owner:** Technical Marketing PIC

### Agentic Workflow
* **Plan:** [How will you orchestrate your tools?]
    * *Example:* "We are using Cursor as the IDE. We have created a `skills.md` file containing the CUDA-Q documentation so the agent doesn't hallucinate API calls. The QA Lead runs the tests, and if they fail, pastes the error log back into the Agent to refactor."

### Success Metrics
* **Metric 1 (Approximation):** [e.g., Target Ratio > 0.9 for N=30]
* **Metric 2 (Speedup):** [e.g., 10x speedup over the CPU-only Tutorial baseline]
* **Metric 3 (Scale):** [e.g., Successfully run a simulation for N=40]

### Visualization Plan
* **Plot 1:** [e.g., "Time-to-Solution vs. Problem Size (N)" comparing CPU vs. GPU]
* **Plot 2:** [e.g., "Convergence Rate" (Energy vs. Iteration count) for the Quantum Seed vs. Random Seed]

---

## 6. Resource Management Plan
**Owner:** GPU Acceleration PIC 

* **Plan:** [How will you avoid burning all your credits?]
    * *Example:* "We will develop entirely on Qbraid (CPU) until the unit tests pass. We will then spin up a cheap L4 instance on Brev for porting. We will only spin up the expensive A100 instance for the final 2 hours of benchmarking."
    * *Example:* "The GPU Acceleration PIC is responsible for manually shutting down the Brev instance whenever the team takes a meal break."
