

# Artifact

- [ ] Registration deadline : 2024. 03. 04
- [ ] Full artifact deadline : 2024. 03. 11


# Revision

- [x] line 40-41: "It does not take..." - awkward grammar 
- [x] Move Fig 4 and 5 closer to where they are referenced
- [x] line 726: "classificaiton"
- [x] Line 357, "it return" -> "it returns"
- [x] We will revise the Introduction (Section 1) to clarify that the two metrics are proxy metrics.
- [x] To prevent the confusion, we will clarify the meaning of the terms top-down and bottom-up in the revised paper.

- [x] In the revised paper, we will include intuitive explanations to help understand the motivation behind Fidelity. The metric Fidelity is designed to measure the faithfulness of the subgraph explanations provided by model explanation techniques. The insight of the metric Fidelity is that if a provided explanation subgraph is the actual reason for the original prediction, the model should classify the subgraph into the same label.

- [x] Using a deeper `chosen_depth` may enable our algorithm to synthesize better GDL programs. However, a larger `chosen_depth` significantly increases the learning cost; therefore, we used a smaller `chosen_depth` for complex datasets. This implementation detail was omitted in the submitted paper due to the space limit. We will clarify this in the revised paper.

- [x] We will try to include qualitative analysis for the 5 datasets in the revised paper.

- [x] We will discuss the expressiveness in the revised paper. In terms of expressiveness, GDL is strictly more expressive than subgraphs as a subgraph can be represented by a GDL program, but not vice versa.




# Revise Limitation to include:

- [x]  Trade-offs of PL4XGL given its potential higher training cost and expressiveness compared to GNNs (Reviewer A).

- [x]  Two fundamental limitations: (1) current GDL is very simple, which precludes many use-cases, and (2) PL4XGL cannot support regression (Reviewer B).

- [x] PL4XGL may not be the best choice for applications where the reliability of explanations is not a critical concern (Reviewer C).

- [x] Currently, only a small fraction of the learned GDL programs are actually used for the classification task; applying model minimization techniques can address this problem (Reviewer C).

- [x] The current approach may not be effective in datasets with class imbalance (Reviewer C).

- [x] The current synthesis algorithm may not find optimal GDL programs (Reviewer D).

- [x] If the model (PL4XGL) includes a large number of features (GDL programs) and the graph is dense, the current PL4XGL may not be scalable (i.e., it may require a significant amount of time to make predictions). However, this is not a fundamental limitation of PL4XGL.  For example, this limitation can be addressed by applying model minimization techniques [1] to reduce the number of features in the learned model. This will be discussed in the revised paper.

----------------
- [ ] We will include large datasets that clearly show the limitations of PL4XGL in terms of training cost. For example, we will try to find large datasets that PL4XGL fails to finish its training, while GNNs finish their training.

- [ ] We will include another GNN explanation technique in the revised paper.

- [ ] To address this concern, we will revise Figure 9 to show the three types of cost separately.
 

<div style="page-break-after: always;"></div>




**Mandatory revisions**

1. Incorporate rebuttal content into the paper:

- [ ]  limitation & future work section (expressiveness, scalability, classification only)
- [ ]  experiment section: revise figure 9 for better cost presentation; explain fidelity better; add qualitative analysis
- [x]  elaborate search depth in the technical section
- [ ]  compare with subgraph based approach (in related work)

2. Revise intro/overview for better framing:

- [ ]   better define scope and expressiveness, clarify "correctness of explanations"

**Optional revisions**

- [ ]  Evaluate on large dataset from SubgraphX
- [ ]  Evaluate with different search depth to show tradeoff between performance and time
- [ ]  Incorporate the example in rebuttal comparing GDL and subgraphs in the paper, this example will also help general audience to understand the difference better