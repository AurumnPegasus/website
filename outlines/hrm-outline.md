# Outline: Math behind HRM (Hierarchical Reasoning Model)

> Note: This is the literal SERP-informed outline you asked for. It optimizes for
> search visibility, which pulls against your blog's voice rules (first-person,
> anti-SEO, story-driven). Where the SEO playbook and your voice collide, your
> voice should win on the actual post. The question-format headings and FAQ
> section below are SEO conventions, not your usual `## Core Motivation` style.

## Title Suggestions
1. The Math Behind HRM: Reasoning Without BPTT (52 chars)
2. How HRM Skips Backprop Through Time (Explained) (47 chars)
3. What Makes the Hierarchical Reasoning Model Work? (49 chars)

## Target Parameters
- **Primary keyword**: hierarchical reasoning model math / HRM BPTT
- **Secondary**: one-step gradient approximation, deep equilibrium model, HRM architecture
- **Search intent**: Informational (technical practitioners, ML researchers)
- **Target word count**: ~2,200 words (sweet spot: shorter than gonzoml's 5k, deeper math than aipapersacademy's 1.8k)
- **H2 sections**: 7
- **Target reading level**: technical audience, no Flesch target (your audience is researchers, per CLAUDE.md)

---

## Outline

### H2: Why transformers run out of depth (~300 words)
- **Answer-first opener**: Transformers are single-shot. They have fixed computational depth, and CoT is a workaround, not real depth.
- **Key points to cover**:
  - Shallowness of transformers / LLMs, single forward pass
  - CoT depends on human-defined decompositions; one bad block stalls the whole pass
  - Latent reasoning attempts (COCONUT / Chain of Continuous Thought) and why they fall short
- **Key statistic to find**: HRM hits 40.3% on ARC-AGI vs o3-mini-high 34.5%, Claude 3.7 21.2%, at 27M params / ~1000 samples
- **Chart suggestion**: Bar — HRM vs o3-mini vs Claude 3.7 on ARC-AGI
- **Image placement**: No (lead with prose)
- **Voice note**: This maps to your existing `## Core Motivation`. Keep your phrasing.

### H2: What is the Hierarchical Reasoning Model? (~250 words)
- **Answer-first opener**: HRM is a recurrent architecture with two coupled modules at different timescales, attaining depth without the BPTT cost.
- **Key points to cover**:
  - High-Level RNN (slow, abstract planning) + Low-Level RNN (fast, detailed compute)
  - H updates at a modulo of L updates (brain-wave frequency inspiration)
  - Each "RNN" cell is itself a transformer block, not vanilla RNN/LSTM
- **Chart suggestion**: None
- **Image placement**: Yes — your `slow_fast.png` (slow-fast brain waves) goes here
- **Voice note**: This is your existing closing paragraph, expanded.

### H2: Why stacking layers naively fails (~250 words)
- **Answer-first opener**: Depth via more layers hits vanishing gradients and kills parallelism; this is the RNN/BPTT problem.
- **Key points to cover**:
  - Vanishing gradients, training instability (the RNN curse)
  - BPTT is O(T) memory, no parallelization in forward or backward
  - This is the cost HRM is trying to dodge
- **H3: What BPTT actually costs**
  - Unrolling through time, memory scaling, why it is biologically implausible
- **Key statistic to find**: O(T) → O(1) memory reduction (the headline claim)
- **Chart suggestion**: None (or a small line: memory vs sequence length, BPTT vs HRM)

### H2: How does HRM avoid backprop through time? (~400 words) [CORNERSTONE]
- **Answer-first opener**: HRM uses a one-step gradient approximation grounded in Deep Equilibrium Models, computing gradients only from the final converged states.
- **Key points to cover**:
  - Deep Equilibrium Models (Bai et al., 2019) connection
  - Fixed-point convergence: once the recurrence converges, you can backprop at the equilibrium point only
  - Implicit Function Theorem (IFT) gives the gradient without unrolling
  - The 1-step approximation: replace `(I − J_F)⁻¹` with `I`
- **H3: The math, step by step**
  - The equilibrium equation, IFT statement, the Jacobian-to-identity simplification
- **Key statistic to find**: confirm the exact one-step gradient formulation from the arXiv paper (2506.21734)
- **Chart suggestion**: None — use a `{{< highlight >}}` block or LaTeX for equations
- **Voice note**: This is the heart of the post ("they claimed to find a way around BPTT"). Most depth goes here.

### H2: Hierarchical convergence — the part that makes it work (~300 words)
- **Answer-first opener**: L converges to a local equilibrium each cycle; then H updates and resets L's context, forcing a fresh convergence. Nested computation without one global fixed point.
- **Key points to cover**:
  - L finds a local equilibrium within a cycle
  - H reads L's final state, updates, re-sets L's context
  - Many distinct nested computations = effective depth
  - Why this avoids the "early convergence" failure of plain DEQ
- **Chart suggestion**: None
- **Image placement**: Yes — a cycle/convergence diagram (you may need to make one)

### H2: Adaptive Computation Time — thinking only as long as needed (~250 words)
- **Answer-first opener**: HRM decides per-problem how many cycles to run, via a Q-learning halting head. Easy problems halt early, hard ones run deeper.
- **Key points to cover**:
  - ACT / adaptive halting mechanism
  - Q-learning for the halt/continue decision
  - Deep supervision across segments
- **Chart suggestion**: None
- **Voice note**: Optional. Cut if the post is getting long — your call.

### H2: What I think HRM gets right (and what I am unsure about) (~250 words)
- **Answer-first opener**: HRM's real contribution is recursive latent depth at O(1) memory, independent of the bio-inspiration story.
- **Key points to cover**:
  - The bio-inspiration may or may not matter; the BPTT dodge is the substance
  - Open questions / where you might be wrong (you flagged this in the draft)
  - ARC-AGI post-analysis caveats (ARC Prize found the gains came partly from other drivers)
- **Key statistic to find**: ARC Prize analysis findings on what actually drove HRM's score
- **Voice note**: This replaces the SEO "Conclusion." It is your honest-uncertainty close, which fits your voice. Keep the "mail me if I am wrong" line.

---

### FAQ Section (skip unless you want the SEO schema — does not fit your voice)
1. Does HRM use backpropagation through time? — No; one-step gradient approximation via DEQ.
2. How many parameters does HRM have? — ~27M, trained on ~1000 samples.
3. What is the difference between the high-level and low-level module? — Timescale: slow abstract planner vs fast executor.
4. Is HRM actually brain-inspired? — Loosely; the timescale separation echoes brain-wave frequencies, but the core win is computational.

---

## Internal Linking Zones
- **Link TO from this post**: COCONUT paper (already linked); the original HRM arXiv (2506.21734); DEQ paper (Bai et al. 2019)
- **Link FROM to this post**: any future post on latent reasoning, RNNs, or your NLP/LSTM background; the sigmoid post if it touches gradients

## Content Gaps to Exploit
1. **Worked one-step-gradient derivation.** aipapersacademy (1.8k words) stays conceptual with "minimal equations"; gonzoml has the math but buries it in 5k words of history. A tight, correct derivation of the `(I − J_F)⁻¹ ≈ I` step is the gap.
2. **Honest "where this might be wrong" framing.** Every competitor is explainer-neutral or hype-leaning. Your first-person "I inferred this, correct me" angle is genuinely differentiated and AI-overviews tend to surface confident-but-hedged primary takes.
3. **The COCONUT contrast.** Most pieces mention latent reasoning generically; you already frame HRM against COCONUT specifically. Lean into that comparison — it is a real information-gain angle.
4. **ARC Prize's skeptical post-analysis.** Few explainers cite the finding that HRM's ARC gains came partly from non-architectural drivers. Including it is both honest and a citation magnet.

---

## Sources used for SERP analysis
- [HRM paper (arXiv 2506.21734)](https://arxiv.org/html/2506.21734v1)
- [Gonzo ML deep-dive](https://gonzoml.substack.com/p/hierarchical-reasoning-model)
- [AI Papers Academy](https://aipapersacademy.com/hierarchical-reasoning-model/)
- [ARC Prize HRM analysis](https://arcprize.org/blog/hrm-analysis)
- [IBM: What is HRM](https://www.ibm.com/think/topics/hierarchical-reasoning-model)
