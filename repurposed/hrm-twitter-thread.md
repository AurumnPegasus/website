# HRM Math — Twitter/X Article

Source: content/posts/hrm.md — "Understanding the math behind HRM"
Format: long-form Twitter Article (single piece, headline + body), not a thread

---

## Title

The math behind HRM, and the one approximation that makes it cheap to train

---

## Body

One of my favorite papers is HRM by Sapient Intelligence. I dug into its math a while back, mostly because two things pulled at me. A lot of my early NLP days were spent on recursive modules like LSTMs, which do not find much use nowadays. And HRM claims bio inspiration, which I am fond of. I presented this to my cofounders about half a year ago, and I wanted an online record of it.

Transformers, and the LLMs built on them, are inherently shallow. Computation happens in a single forward pass of fixed depth, so there is a hard ceiling on how much thinking can happen before an answer has to come out. Chain of Thought is a way around this, but it depends on human defined reasoning steps, and any mistake leaves you stuck with that block for the whole pass. It is a workaround, not a solution.

The interesting question is why depth matters and width does not. On Sudoku-Extreme, which needs extensive tree search and backtracking, making a Transformer wider yields no gain. Making it deeper is critical. The problem is that naively stacking layers brings back vanishing gradients and the cost of backpropagation through time. RNNs already taught us this lesson.

HRM's structure is simple. Two recursive units, each of which is itself a Transformer. A low-level RNN unrolls for N steps, then a high-level RNN updates, and this repeats T times. There is not much to think about in the architecture. The math of training it is where it gets interesting.

Consider an idealised version where the low-level state converges to a fixed point before each high-level update. That lets you compress the whole network into a single function where the high-level state maps to itself.

    z_H* = F(z_H*, x; θ)

A state that is a fixed point of its own update. Once you have that, the Implicit Function Theorem applies. It tells you how the fixed point changes with the parameters θ without unrolling the recursion at all. You differentiate the fixed-point equation in place. This is exactly the term BPTT normally pays a large price to compute.

The gradient the theorem gives you contains an inverse matrix term, (I − J)⁻¹, where J is the Jacobian of F. Inverting that looks expensive. The Neumann series rewrites it as a sum of powers of J.

    (I − J)⁻¹ = Σ Jᵏ

Now the real question is how many terms you actually need. This is the part I liked most. At convergence the state deltas go to zero, and that forces J to have norm less than one. So every higher power of J shrinks toward nothing. You keep only the k=0 term and drop the rest. This is an approximation, not an assumption, and convergence is what justifies it. BPTT collapses to a single step.

Two pieces sit on top of this. Deep Supervision iterates the network M times and detaches the hidden state between segments, which is the same 1-step approximation applied again. Adaptive Computational Time uses a small DQN to decide when to halt, so easy samples stop early instead of burning through more iterations.

That is the spine of HRM. A fixed point, the Implicit Function Theorem, a Neumann series that collapses to one term, and a supervision scheme built on top of the cheap gradient.

The full write-up has every equation, including the gradient approximation step by step and the ACT targets.

Full derivation: https://aurumnpegasus.com/posts/hrm/

If I got something wrong, please email me so I can correct it.

---

## Notes

- This is the Twitter long-form Article format: one headline plus flowing body, not numbered tweets.
- Voice kept to the post: first person, measured, no em dashes, no contractions in the argument sections, no CTA beyond the post's own "email me" line.
- No fabricated statistics. The source is a math derivation with no stat/source data points.
- Math in plain text/Unicode since the Article editor does not render LaTeX. Approximations of the post's notation.
- Confirm the URL slug /posts/hrm/ matches the published path.
