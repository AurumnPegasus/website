---
title: "The Geocentrism of Neural Networks"
date: 2026-04-24T12:00:00+05:30
draft: false
tags:
  - ml
  - neuroscience
  - research
---

We do not know what intelligent machines look like, so we copy the closest one we have. Sort of like virtue ethics: in the absence of a formal definition, you imitate what seems to work.

My hypothesis is that this is analogous to geocentrism. A framework that explains some observations, works pretty well for practical tasks, but builds on fundamentally incorrect foundations. And just like geocentrism, it leads to a ceiling. Iteration after iteration to explain things that a different foundation would make simpler. 

## Where this started

I was reading [Continuous Thought Machine][ctm]. CTM builds much of its hypothesis on "neural synchrony", the temporal coordination of neural activity across different neurons or brain regions. Main idea: the timing of when neurons fire relative to each other carries important information in their communication. This is grounded in the binding by synchrony theory by Singer ([Singer & Gray, 1995][singer-gray]; [Gray, 1999][gray-1999]). 

The problem is that binding by synchrony is, from my understanding, debatable at best within neuroscience communities. 

- **Shadlen, M. N., & Movshon, J. A. (1999). ["Synchrony unbound: A critical evaluation of the temporal binding hypothesis."][shadlen-movshon] *Neuron*, 24, 67-77, 111-125.** 
    - *Core arguments:* (1) No biophysical evidence neurons are selective to synchronous input at this precision; (2) cortical activity with such precise synchrony is rare; (3) unclear how rate and synchrony could be independently interpreted.
- **[Thiele, A., & Stoner, G. (2003).][thiele-stoner]** Found that perceptual binding of two moving patterns (coherent vs. noncoherent plaids) had NO effect on synchronization of the responding neurons. *Direct empirical failure.*
- **[Goldfarb, L., & Treisman, A. (2013).][goldfarb-treisman]** Logical problem: if you have a red square AND a red circle, "red" needs to synchronize with two objects simultaneously. The scheme becomes computationally awkward.
- **[Dong et al. (2008).][dong-et-al]** Found synchrony is independent of binding condition in V1.

To be honest, I found the initial overview on Reddit thread, and then I verified using Claude. I do not claim to be a neuroscience / biology expert, but these links themselves do tell me that neural synchrony is a questionable foundation.

$$
S^t = Z^t \dot (Z^t)^T \in \R^{D \times t}
$$

What makes it worse is how CTM actually implements "neural synchrony". It defines synchrony as a dot product over post-activation histories ([CTM technical overview][ctm-site]). Two neurons that activated high at the same timesteps and low at the same timesteps get high synchrony scores. That is a reasonable engineering choice, but its connection to the neuroscience literature on binding by synchrony is thin. Maybe it is expected information loss when translating biological concepts to computation. To me, it seemed more like retrofitting to make it work (which is becoming way too common).

## Previous Papers

The reason I went to this depth was [Hierarchical Reasoning Model][hrm].
HRM is a genuinely good paper, and Sapient as a company has built their identity around neuroscience-reasoning integration. The paper references lower level and higher level frequency waves as part of their biological grounding.

But as I went deeper into the actual mechanism, what mattered end of the day were two things:

1. Backpropagation through time approximation
2. Increased recurrence due to (1)

The whole idea of different frequency waves was not the reason the model worked. The [Tiny Recursive Model][trm] (TRM) paper makes this clearer. TRM strips away the neuroscience framing and demonstrates that the core contribution is increasing recursive depth. The biological story was a loosely connected thread over a computational insight.

Another example. Prioritised experience replay ([Schaul et al., 2015][per]) in reinforcement learning is inspired by the hippocampus of rodents. The neuroscience suggests that sequences of prior experiences are replayed during rest, and sequences associated with rewards are replayed more frequently ([Foster & Wilson, 2006][foster-wilson]; [Ambrose et al., 2016][ambrose-et-al]). This maps neatly onto the idea of a replay buffer (store and reuse past experiences) and prioritisation (replay important ones more).

Except they do not use priority based on reward. They use temporal difference error, which is the difference between the current prediction and the next step prediction. The prioritisation mechanism has zero connection to the biological justification the paper presents. The hippocampus story is a narrative wrapper around a mathematically motivated sampling strategy.

In the original paper by [Lin][lin-1992], the reason for experienced replay was not biological link, but the idea that since RL data stream is breaking the IID assumption, we should collect then randomly sample so as to get pseudo-iid data for network to train on.

## The Irony

This connects to Sutton's [Bitter Lesson][bitter-lesson]: general methods that leverage computation tend to win over methods that leverage human knowledge about the domain. But I think the point is not just about compute. It is also about representation.

KGs are not stored like anything in our brain. They are a database-inspired relationship representation. But they work well because they are truthful to the mathematics of relational structure, not because they mimic biology.

TRM works not because it abandoned neuroscience entirely, but because it was honest about what the computational mechanism actually was, increased recursive depth, rather than wrapping it in a biological story about frequency waves.

NEAT ([Stanley & Miikkulainen, 2002][neat]) is biologically inspired in a more literal sense, evolving network topology through genetic algorithms the way natural selection shapes neural circuits. But it consistently underperforms dense, fully connected networks with sufficient compute. The biologically plausible structural prior loses to the mathematically simpler brute force approach.

Being strongly connected and truthful to the underlying mathematics, as TRM is, tends to win over being weakly inspired by correct or incorrect biology, as HRM is. This is my secondary hypothesis: it is not just that more compute wins, it is that cleaner, more honest representations win.

## The Ceiling

So where does this leave us?

While there are people bringing up vague biology to support their results, I feel AI has become more math-rigorous field than it used to be. This is for the better, since now the designs and inspirations are honest about how they want to solve the intelligence problem. Still, to make current work break the ceiling, we need to reinterpret current results through a different lens, perhaps information theory, network connectivity theory, optimisation problems. We need to find a framework which actually explains why current architecture works (this might lead to finding multiple frameworks since AI has become such a vast and varied field). 

The second is to start from scratch. Build new systems from first principles with a different foundational interpretation. Not necessarily abandoning biological inspiration, but demanding that the connection be truly explainable throughout, not loosely motivational. This will not produce state of the art results immediately. Whoever starts down this path will be far behind, and scaling existing neural networks will continue to outperform them for a long time.

I lean strongly toward the second path.

This feels analogous to the connectionism versus symbolic AI debate before neural networks became dominant. Connectivists were doing poorly for a long time, especially when the symbolists had [ELIZA][eliza] and expert systems like [MYCIN][mycin] that worked for practical tasks. But the connectivists were building on a foundation that eventually proved more general.

I do not know what the heliocentrism of deep learning looks like. It could be a deeper, more rigorous engagement with neuroscience. It could be information theory. It could be something we have not thought of yet. What I do believe is that the current practice of loosely borrowing neuroscience terminology to justify computational design choices, while the actual performance comes from mathematical properties that have nothing to do with the biological story, is a ceiling we will eventually hit. And when we do, it will be because we spent too long adding iterations instead of questioning the foundation.

## References

- [Ambrose, Pfeiffer, and Foster (2016), "Reverse Replay of Hippocampal Place Cells Is Uniquely Modulated by Changing Reward"][ambrose-et-al]
- [Darlow, Regan, Risi, Seely, and Jones (2025), *Continuous Thought Machines*][ctm]
- [Darlow et al. (2025), CTM technical overview][ctm-site]
- [Dong, Mihalas, Qiu, von der Heydt, and Niebur (2008), "Synchrony and the Binding Problem in Macaque Visual Cortex"][dong-et-al]
- [Foster and Wilson (2006), "Reverse Replay of Behavioural Sequences in Hippocampal Place Cells During the Awake State"][foster-wilson]
- [Goldfarb and Treisman (2013), "Counting Multidimensional Objects: Implications for the Neural-Synchrony Theory"][goldfarb-treisman]
- [Gray (1999), "The Temporal Correlation Hypothesis of Visual Feature Integration: Still Alive and Well"][gray-1999]
- [Hogan et al. (2021), "Knowledge Graphs"][knowledge-graphs]
- [Jolicoeur-Martineau (2025), "Less is More: Recursive Reasoning with Tiny Networks"][trm]
- [Lin (1992), "Self-Improving Reactive Agents Based on Reinforcement Learning, Planning and Teaching"][lin-1992]
- [Mnih et al. (2015), "Human-Level Control Through Deep Reinforcement Learning"][dqn]
- [Schaul, Quan, Antonoglou, and Silver (2015), "Prioritized Experience Replay"][per]
- [Shadlen and Movshon (1999), "Synchrony Unbound: A Critical Evaluation of the Temporal Binding Hypothesis"][shadlen-movshon]
- [Singer and Gray (1995), "Visual Feature Integration and the Temporal Correlation Hypothesis"][singer-gray]
- [Stanley and Miikkulainen (2002), "Evolving Neural Networks Through Augmenting Topologies"][neat]
- [Sutton (2019), "The Bitter Lesson"][bitter-lesson]
- [Thiele and Stoner (2003), "Neuronal Synchrony Does Not Correlate With Motion Coherence in Cortical Area MT"][thiele-stoner]
- [Wang et al. (2025), *Hierarchical Reasoning Model*][hrm]
- [Weizenbaum (1966), "ELIZA: A Computer Program for the Study of Natural Language Communication Between Man and Machine"][eliza]
- [Buchanan and Shortliffe (1984), *Rule-Based Expert Systems: The MYCIN Experiments of the Stanford Heuristic Programming Project*][mycin]

[ambrose-et-al]: https://pubmed.ncbi.nlm.nih.gov/27568518/
[bitter-lesson]: http://www.incompleteideas.net/IncIdeas/BitterLesson.html
[ctm]: https://arxiv.org/abs/2505.05522
[ctm-site]: https://pub.sakana.ai/ctm/
[dong-et-al]: https://pmc.ncbi.nlm.nih.gov/articles/PMC2647779/
[dqn]: https://www.nature.com/articles/nature14236
[eliza]: https://doi.org/10.1145/365153.365168
[foster-wilson]: https://www.nature.com/articles/nature04587
[goldfarb-treisman]: https://pubmed.ncbi.nlm.nih.gov/23334446/
[gray-1999]: https://pubmed.ncbi.nlm.nih.gov/10677025/
[hrm]: https://arxiv.org/abs/2506.21734
[knowledge-graphs]: https://doi.org/10.1145/3447772
[lin-1992]: https://doi.org/10.1023/A:1022628806385
[mycin]: https://shortliffe.net/Buchanan-Shortliffe-1984/MYCIN%20Book.htm
[neat]: https://doi.org/10.1162/106365602320169811
[per]: https://arxiv.org/abs/1511.05952
[shadlen-movshon]: https://doi.org/10.1016/S0896-6273(00)80822-3
[singer-gray]: https://pubmed.ncbi.nlm.nih.gov/7605074/
[thiele-stoner]: https://pubmed.ncbi.nlm.nih.gov/12540900/
[trm]: https://arxiv.org/abs/2510.04871
