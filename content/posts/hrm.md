---
title: "Math behind HRM"
date: 2026-06-15T12:00:06+09:00
draft: true
tags:
  - ml
  - math
  - paper
---

One of my favorite papers was HRM by Sapient Inc., and I had long ago gone in depth of its math (since they claimed to find a way around BPTT). I loved this since a) a lot of my initial NLP days was figuring out recursive modules like lstms which dont find much use nowadays and b) they claimed bio inspiration which I am fond of. I presented this to my cofounders half a year back, thought I would revise and mantain an online record of it.

This blog is just an understanding of the math and motivation I have inferred from the contents of the paper. If I am wrong in any place, please mail me so I can correct myself.

## Core Motivation

The main motivation begind HRM is the inherent shallowness of transformers and LLMs. They do not have (enough) computational depth. Afterall, its a single shot thing. A way around this was Chain of Thought (CoT) prompting for reasoning. But, end of the day, its just a way around, not a proper solid thinking process. It depends on human defined decompositions where a single mistake means that you are stuck with that block for the whole pass. 

Towards this goal, there have been people exploring latent reasoning. One of the most famous is [COCOCNUT](https://arxiv.org/abs/2412.06769) (Chain of Continuous Thought) paper, which replaces the tokens generated in CoT with just the latent representation via thought tokens. Ultimately, this too is not good enough. 

Naively stacking layers is notoriously difficult due to vanishing gradients, which plague training stability and effectiveness (something which RNNs faced). It is also insanely expensive computationally due to lack of parallelisation possible, both in forward pass and in backward. This is due to BPTT (backpropagation through time). 

HRM claims to be biologically inspired, and it might well be, but the main problem that it solves is to allow for recursive thought depth in the latent space while not paying the BPTT cost. By doing just that much, you are able to train it over larger sequences, mantain coherence and training signals throughout it. This allows for models to perform more complex tasks.

Inspired by brain wave frequencies, there is a High-Level RNN and a Low-Level RNN. The lower level updates at each time step, and higher level updates at some modulo of lower level updates. Interestingly, each NN in the RNN itself is a transformer block, which itself has a lot more reasoning capabilities than vanilla RNN or LSTMs


![slow-fast brain waves](/images/hrm/slow_fast.png)

