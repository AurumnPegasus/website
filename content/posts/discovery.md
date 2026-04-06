---
title: "Discovery is Not Search"
date: 2026-04-06T12:00:00+05:30
draft: false
tags:
  - theatre
  - research
  - startup
---

Around this time, a year ago, I made the decision to leave Salesforce. 

Because I was bored. As a company, I was happy with the culture, with the people and policies at Salesforce. But I was just bored because I was not solving problems that I felt were challenging or meaningful enough for me. I was working on an agentic use case for which systems were already designed, for users I have never met, in a domain I had no connection with. I do understand why. The problems given to freshers are often constrained consciously, to let them learn within structure. But it made me feel as if I was engineering in a vacuum, just for the sake of it.

The other thing was, and this is obviously a common sentiment now, Claude was already a better executor than me for most of what I was doing. Especially at the bounded, well defined tasks that I was getting. The combination of me being a decent engineer and Claude being a great executor meant that I finished my tasks too quickly (which also led to lots of procrastination). There wasn't enough friction.

What made it worse was I had arrived from a place where I constantly faced real friction. Most of my time at college had gone into research, multi-lingual NLP primarily, where I had to struggle and challenge myself to even decide on a single problem statement.

## What research actually felt like

In my limited experience of academia, I could break down the work into roughly three phases: finding the problem statement, consuming surrounding literature to generate a hypothesis, and running experiments to verify it. Of these, for me, finding the correct problem was always the hardest, probably due to lesser experience. The domains often are so vast and open-ended (mine was enhancing Wikipedia for low-resource languages) that narrowing down to a small, real problem often felt like a goldilocks problem. Something doable but meaningful was the perfect recipe everyone was looking for.

Literature review was the fun part for me. I had figured out how to read papers either fast or in depth, and was using this to cover larger areas while going deeper into papers I identified as important. Sometimes you find connections. Paper A building from papers B, C, D that you read earlier. Reading papers on Grokking was such an interesting experience because of this.

The boring version of lit review was comprehensive search. The grind of ensuring you have covered enough ground, that sources are from reputable conferences or labs, while also checking if some less-reputed paper had a reproducible idea worth working on.

Experimentation was generally straightforward. It felt more like engineering than research, and back when claude did not exist, it was tougher than it is right now. It still had its fun, but I wont go much into it because it is something which everyone does, whether in academia or not. 

## The FSA Story

In my first year, I studied context-free grammar and decision trees, as a part of my linguistics course to observe how some grammatical rules can be encoded via it. In my second year, I studied finite state automata, building on state transitions, deterministic vs non deterministic, formal languages and so on. In both cases, I found it pretty interesting but also as more of a foundation than something people realistically use.

Sometime later in my RL course, I saw it again, in context of modelling environments with discrete state spaces. Again, as a small footnote, as a base to understand more complex models.

Then, I started working on [Wikipedia Outline Generation](https://dl.acm.org/doi/10.1007/978-3-031-78538-2_13) (OutlineGen). The core problem was to generate an outline (table of contents) for a Wikipedia article in low-resource languages, building on top of another Wikipedia-related paper I had earlier published, [XWikiGen](https://dl.acm.org/doi/pdf/10.1145/3543507.3583405). It seemed like a straightforward task. I looked into research related to text generation, maybe grounding + generation if you are being fancy, or table-of-contents generation if you were pedantic. Most of my friends / lab-mates framed it that way, and we began our experimentation.

The more I did those experiments, I just saw such poor results. I went through the data, and I found out the reason. Wikipedia articles (especially in low-resource languages) are so poorly represented that they often have similar outlines for most articles. This leads to a long-tail distribution problem with a lot of repetition, and there was simply not enough variance in the data for a generation task. The repeated structural patterns meant that I could construct a transition matrix, essentially an FSA, to determine the correct outline. What looked like a generation problem was actually a much simpler state transition problem.

I did not find this idea by looking for "FSA for text generation". No search query in the world could have gotten me here. The connection happened because other people in my lab were presenting their work on Deep Q-Learning with Transformers, which reminded me of RL, which reminded me of FSA, which clicked with the structure I was seeing in my data. It was a chain of loosely related ideas from different domains that converged on a solution.

This is what I mean by discovery, and why it is fundamentally different from search.

## Search and Discovery

Search is looking for something similar. Discovery is finding something that fits a hole.

Search is an optimised problem. Google is extraordinarily good at it (although dead-internet theory has made it so much worse). Semantic Scholar, arXiv search, even Perplexity, would give you good results which are similar to your query. If you know what you are looking for, search works.

Discovery is what happens when you don't know what you are looking for. When you have a problem, not a query, and you need to find ideas that fit a gap you can not perfectly articulate. The FSA insight didn't come from a query. It came from a different framing of the problem that revealed the right solution.

There is massive overlap between the two, and I want to be honest about that. Discovery still needs search. You still need retrieval, ranking, filtering. But discovery adds something on top, whether that be reasoning, problem decomposition, or cross-domain exploration. The ability to take your problem and convert it into multiple possible interpretations, then explore literature for each interpretation to validate them.

## Current Bottleneck

Here is why I think discovery is a more important problem to solve right now.

The obvious answer is "experimentation is where the real work happens", and historically, that is true. Writing code, running experiments and ablations, debugging, that was the time sink. But that equation has changed. Claude Code / Cursor etc have compressed the experimentation cycle dramatically. Code is cheaper to write now. It is cheaper to throw away. I spent weeks building an RL-based transformer system with carefully designed manual rewards for language generation. It performed worse than the simple FSA approach. That hurt, and I spent months trying to debug and improve it because of the time I had sunk in. Either I was young and stupid, or too attached to the effort I had put in. Not the best decision making, but still. Today, I could prototype both approaches in a week and compare results before committing. 

So if experimentation is faster, what determines whether you waste a month or ship a result?

Discovery is part of the "knowing what to do" problem. It helps you identify which experiments are worth running, which framing of your problem is most promising, which existing solutions from adjacent domains might transfer to yours.

Here is where I want Theatre to go: when a researcher describes a problem, the system should be able to break it into 3-4 different interpretations or subproblems. Not just "here are papers about Wiki outline generation" but "this could be framed as a generation problem, grounding problem, seq2seq problem, or constrained state transition problem, and here is literature for each framing."

Take my other project, XWikiGen: cross lingual Wikipedia text generation using citations. The obvious framing is context management: you have citations, you need to generate coherent text from them. But it could also be framed as a grounding problem, or a citation-conditioned generation problem (which is what worked), or even a multi-document summarisation problem (which did not work, but was a legitimate thread).

## What discovery is not

I want to close by being clear about what I am not claiming.

I am not claiming discovery replaces the hard work of research. Reading papers, forming hypotheses, designing experiments, all are still extremely important hard work. I am not claiming that Theatre will have a "eureka" button that solves your problem for you.

I am also not claiming that search does not matter. Search is the foundation. You need excellent retrieval and ranking before you can do anything else. The argument is not that search is solved and we should move past it. It is that search alone is not sufficient, and the layer above it (reasoning, decomposition, cross-domain exploration) is where the real value lies.

What I am claiming is this: The bottleneck in research has shifted. Code is faster. Writing is faster. Failing is faster. But figuring out what to code and what to write is still slow, and is tougher now due to more unreliable, non-reproducible, non-peer-reviewed literature out there.
