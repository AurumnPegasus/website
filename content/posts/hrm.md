---
title: "Understanding the math behind HRM"
date: 2026-06-15T12:00:06+09:00
draft: false
tags:
  - ml
  - math
  - paper
---

One of my favorite papers is HRM by Sapient Intelligence, and I dug deep into its math a while back. I found two things interesting, first: a lot of my early NLP days was figuring out recursive modules like lstms, which dont find much use nowadays; second: they claimed bio inspiration which I am fond of.

About half a year back, I presented this to my cofounders. I thought it would be nice to revise, and have an online record of it. I plan to focus on the motivation for it, and the core math as inferred from the contents of the paper.

If I have gotten something wrong, please email me so I can correct it.

## Why Transformers are not enough.

Transformers, and the LLMs built on them, are inherently shallow. Computation happens in a single forward pass of fixed depth, so there's a hard ceiling on how much "thinking" can happen before an answer has to come out. It does not perform well enough for tough reasoning tasks.

A way around this is Chain of Thought (CoT) prompting. In this, reasoning follows the same process as humans, writing down step by step the prioirs, reasons, and then the execution. Ultimately, its just a way around the shortcomings, not the best solution.

There are issues with CoT too, it depends on human defined reasoning steps, and any mistake means that you are stuck with that block for the whole pass. Also, tokens are too ineffecient to think in, to which there was [COCONUT](https://arxiv.org/abs/2412.06769) (Chain of Continuous Thought) that replaced tokens with thought tokens. This too is not good enough.

## Why is Computational Depth important?

![computational depth](/images/hrm/depth.png)

In the left-graph, on Sudoku-Extreme Full, which require extensive tree-search and backtracking, increasing a Transformer's width yields no performance gain, while increasing depth is critical. The graph on the right shows how HRM is able to achieve better performance even than recursive transformers, highlighting that there is a ceiling on recurrant transformers.

Naively stacking layers is notoriously poor due to vanishing gradients, which cause poor training stability and ineffectiveness (something which RNNs faced). It is also insanely expensive computationally due to lack of parallelisation possible, both in forward pass and in backward. This is due to BPTT (backpropagation through time).

## HRM Architecture

![architecture](/images/hrm/architecture.png)

There are multiple parts of HRM architecture which I will go one by one. The main one though is the diagram above. The two-layer hierarchical recursive neural network. Here, each recursive unit is in itself a transformer.

The logic is simple, every $N$

$$
a = \lambda
$$
