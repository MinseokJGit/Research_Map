


# Reviewer A (Post rebuttal)

Thanks for the great response. I'm much more comfortable with advocating for paper acceptance given the **detailed scope** and **expressiveness discussion**. **Please mention these discussion early in the paper (intro/overview) to help readers adjust their expectations.**

I also really appreciate the discussion about search depth. It would be great to include these in the evaluation. Ideally, there could be an experiment (with long evaluation time limit) on maybe only 1-2 benchmarks to show the tradeoff between depth vs accuracy. For example, just pick a slightly harder example, and let it run with depth 1, 2, 3, 4, running for 72 hours, and compare their final performance. This would help readers understand where their limitations are with lower depth, and what's the potential when future acceleration is possible.


Thanks for the great work!

#### After Rebuttal

Thank you very much for the response! It has clearly answered my question about the expressiveness of GDL. **Now, I have a better understanding of the difference between GDL programs and subgraphs. I'm happy to bump up my score to 4.**

**Please consider clarifying the expressiveness of GDL in the revision, as well as discussing the properties of the synthesis algorithm, incorporating some qualitative analysis, and adding discussion about limitations.**

### Comments for authors

The PC has decided to conditionally accept your paper; congratulations!

**Mandatory revisions**

1. Incorporate rebuttal content into the paper:

- [x]  limitation & future work section (expressiveness, scalability, classification only)
- [x]  experiment section: revise figure 9 for better cost presentation; explain fidelity better; add qualitative analysis
- [x]  elaborate search depth in the technical section
- [x]  compare with subgraph based approach (in related work)

1. Revise intro/overview for better framing:

- [x]  better define scope and expressiveness, clarify "correctness of explanations"

**Optional revisions**

- [x] Thanks for the great response. I'm much more comfortable with advocating for paper acceptance given the **detailed scope** and expressiveness discussion. **Please mention these discussion early in the paper (intro/overview) to help readers adjust their expectations.**


- [ ]  Evaluate on large dataset from SubgraphX
- [ ]  Evaluate with different search depth to show tradeoff between performance and time
- [x]  Incorporate the example in rebuttal comparing GDL and subgraphs in the paper, this example will also help general audience to understand the difference better
- [ ]  Properties of synthesis algorithm


