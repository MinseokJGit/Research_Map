
We sincerely appreciate the reviewers for their constructive comments. We will incorporate them into our revised paper.


In particular, we will revise Section 7 ("Limitations and Future Work") to include all the limitations and interesting future works pointed out by the reviewers. The revised section will discuss:

+ Trade-offs of PL4XGL given its potential higher training cost and expressiveness compared to GNNs (Reviewer A).
+ Two fundamental limitations: (1) current GDL is very simple, which precludes many use-cases, and (2) PL4XGL cannot support regression (Reviewer B).
+ PL4XGL may not be the best choice for applications where reliable explanations are not essential (Reviewer C).
+ Currently, only a small fraction of the learned GDL programs are actually used for the classification task; applying model minimization techniques can address this problem (Reviewer C).
+ Current approach may not be effective in datasets with class imbalance (Reviewer C).
+ Current synthesis algorithm may not find optimal GDL programs (Reviewer D).
    
We will also incorporate other comments as well into the revised paper.


# Detailed Response

## Reviewer A

[Q] How many graph features (GDL programs) that PL4XGL can handle practically, and what types of graph learning tasks fall into such cases? For example, if a graph learning task requires a densely connected network with a large number of smaller features, can PL4XGL still handles that well? 


If the model (PL4XGL) includes a large number of features (GDL programs) and the graph is dense, the current PL4XGL may not be scalable (i.e., it may require a significant amount of time to make predictions). 
%
However, this is not a fundamental limitation of PL4XGL. 
%
For instance, this issue can be addressed by applying model minimization techniques [1] to reduce the number of features (GDL programs) in the learned model.
%
This will be discussed in the revised paper.



[Q] Depth of graph features. It looks like `chosen_depth` in NodeClassification/synthesis/simple_learn.py ranges form 1-3 for benchmarks based on data_loader. What's the model's scalability with respect to deeper features? (And are deeper features necessary?) It also seems that more complex dataset requires shallower depth, what does this tradeoff means?


Using a bigger `chosen_depth` enumerates additional candidate mutant GDL programs in each iteration of our synthesis algorithm (line 12 of Algorithm 1). 
%
For instance, if the current GDL program is $P$ and chosen_depth is 1, the implementation enumerates the following GDL program as described at line 12 of Algorithm 1:

$Mutate(P)$

If `chosen_depth` is 2, for example, the implementation additionally enumerate following mutated GDL programs:

$Mutate(P) \cup \bigcup_{P'\in Mutate(P)}Mutate(P')$

For a `chosen_depth` of 3, the implementation enumerates the following mutated GDL programs:

$Mutate(P) \cup \bigcup_{P'\in Mutate(P)}Mutate(P') \cup \bigcup_{P''\in \bigcup_{P'\in Mutate(P)}Mutate(P')} Mutate(P'')$

Using a deeper `chosen_depth` may enable our algorithm to synthesize better GDL programs. However, a larger `chosen_depth` significantly increases the learning cost; therefore, we used a smaller `chosen_depth` for complex datasets. This implementation detail was omitted in the submitted paper due to the space limit, but we will clarify this in the revised paper.




[Q] The paper omits datasets that SubgraphX cannot scale up to. But I think it is nice to present the results for large datasets so that the community better understand the scope, scale and types of problems that PL4XGL can handle.

We will try to include evaluation results for clearly showing the limitations of PL4XGL in terms of training cost. 
%
For example, we will try to include evaluation results on large datasets that PL4XGL fails to finish its training, while GNNs successfully finish their training within a time budget.



[2] Related work: PL4XGL reminds me of explainable tree-based learning approaches, and it would nice to include some comparison. The paper would also benefit from comparing with more coarse-grained graph explanation approaches like GNNExplainer (which has lower cost than edge cutting approaches).

We will try to include another GNN explanation technique in our evaluation.

# Reviewer B

The paper has two fundamental limitations: (1) The constraint language on node and edge features (restricted to matching intervals) is very simplistic, and precludes many use-cases, and (2) It can only support classification, rather than regression.

We will discuss the limitations in the revised paper.


# Reviewer C

[Q] The paper is sorely in need of a more elaborate discussion of capabilities and limitations of the technique. Does this mean we should completely switch to PL4XGL for all our graph learning needs? If so, this is a very strong claim; if not, where is the discussion about when it is not suitable?


PL4XGL may not be the best choice in applications where reliable explanations are not a priority (e.g., recommendation system). 
%
In such applications, training cost and accuracy are more important than the cost of explanations and the accuracy of those explanations. 
%
However, PL4XGL is suitable in decision-critical applications such as fraud detection, health care, and drug discovery, where misclassifications may result in medical accidents or wrongful accusations. 
%
In such applications,  users need reliable explanations for understanding the predictions.



[Q] During the computation of the score, I assume the precision is computed only on the set of components for which labels are available in the training data?

Yes.

[Q] Showing the training + classification + explanation time is completely unacceptable. Each of these operations have different purposes: training time is incurred once, classification time a lot more often, and based on the application scenario, only a fraction of inferences may need explanation.

To address this concern, we will try to revise Figure 9 for showing the three kinds of costs separately.


[Q] I am not sure I understand the motivation behind experiment 2 evaluating a metric where PL4XGL is guaranteed to be the best.

The motivation of the metric _Fidelity_, which is a proxy metric of correctness, is to check whether the provided subgraph explanations are faithful with respect to the model (lines 713 - 721). 


[Q] If I understand correctly, running inference on a graph produces all node labels at once with a GNN, while PL4XGL needs you to run it separately for each node? Is this accounted for in the evaluation?

Yes.


[Q] Are the models learned the size of the training data (nodes + edges) in each case? 

The model does not learn size of the training data. However, improving PL4XGL to learn size of the training data is an interesting future work.


[Q] How expensive does the inference get?

The inference cost is small. 
%
The orange lines in Figure 9 present the sum of training cost and inference cost of PL4XGL. 
%
If the training cost (Accumulated cost when # produced explanations = 0) is excluded, inference cost of PL4XGL is less than 10 minutes.



[Q] What fraction of the programs in the model actually triggered on the test set? Are a few programs triggered repeatedly while the rest being useless?

Yes. In MUTAG dataset, for example, PL4XGL used only 5% of the learned GDL programs when classifying the test set.


[Q] One common approach to disjunctive program synthesis is to use a set-cover like algorithm to pick a small number of programs from the generated set of candidate programs both to improve robustness and efficiency of execution (see, e.g., Iyer et al. PLDI 2019). Can the model be minimized here in the same way?


Yes. PL4XGL can be minimized in the same way. 
%
Then, the inference time of PL4XGL will be far more optimized. 
%
We will discuss this in the revised paper.


[Q] The learning method is very suseptible to class imbalance. A reasonably good program for a very large class can dominate the programs for other classes fully even if it always mislabels points from other classes.

We also observed the problem pointed out by the reviewer. 
%
We will discuss this problem with the observed data and discuss a future work. 
%
Designing a better score function, considering class imbalance, can be an interesting future work.
%
We will clarify it in the revised paper.


[Q] The terms top-down and bottom-up are used completely differently from standard program synthesis literature? I am not sure I understand this paper's terminology.

For preventing the confusion, we will clarify how our top-down and bottom-up algorithms are related to the standard top-down and bottom-up program synthesis algorithms.




[Q] How is generality checked for (3) on page 9? Are we evaluating the program on all training data to checking , or doing a static analysis by sub-graph containment?

We evaluate the program on all training data.


# Reviewer D 

[Q] In terms of expressiveness, I was wondering how GDL compares to the subgraphs used for explaining models in prior work. 

In terms of expressiveness, GDL is strictly more expressive than subgraphs as a subgraph can be described by a GDL program, but not vice versa.
%
For example, a subgraph that have two nodes and an edge that connect the two nodes where the nodes and edges have feature $\langle 1.0 \rangle$ (i.e., $G = (V, E, F_V, F_E)$ where $V = \{v_1,v_2\}$, $E = \{(v1,v2)\}, F_V=\langle \langle 1.0 \rangle, \langle 1.0 \rangle\rangle, F_E=\langle \langle 1.0 \rangle\rangle$)
%
Then, the corresponding GDL program is as follows:
```
node x <[1,1]>
node y <[1,1]>
edge (x,y) <[1,1]>
target graph
```

However, the following GDL program cannot be described by a subgraph :
```
node x <[0,10]>
node y <[0,10]>
edge (x,y) <[0,10]>
target graph
```
It is because the nodes and edges in a subgraph have concrete feature values.





[Q] The paper mainly describes the synthesis algorithms (in Section 5) using graphs instead of a program in GDL. What are the advantages of GDL?

The graphs we used in the synthesis algorithms (in Section 5) are graphical representations of GDL programs. For example, the graph at line 532 is the graphical representation of the following GDL program:
```
node a <[1.2, 1.2]>
node b <[0.2, 0.2]>
node c <[0.4, 0.4]>
node d <[0.8, 0.8]>
edge (b,a)
edge (b,c)
edge (c,d)
target node a
```
Due to the space limit, we used the graphical representations of GDL programs. 


[Q] Readers may be curious about the properties of the proposed synthesis algorithm, but the paper does not formalize the soundness, completeness, or other interesting properties such as optimality of the synthesis algorithm.

We will discuss various properties in the revised paper. For example, we will clarify our algorithm may not find optimal GDL programs.


[Q] It would be better if the paper could provide the average running time and the average number of iterations to synthesize an explanation.

We will clarify the average running time and the average number of iterations in the revised paper.


[Q] The notion of "correctness of explanations" is not completely clear until it is explained in Section 6.2. From my understanding, fidelity and sparsity are more like proxy metrics for correctness, which should be clarified and explained more at the beginning of the paper.

We will revise the Introduction (Section 1) to clarify that the two metrics are proxy metrics.

[Q] I am also curious why PL4XGL has better accuracy than baselines on the reset 5 datasets. It would be better if the paper could incorporate some qualitative analysis.

We will include qualitative analysis for the 5 datasets in the revised paper.

# Reference

[1] Arun Iyer, Manohar Jonnalagedda, Suresh Parthasarathy, Arjun Radhakrishna, and Sriram K. Rajamani. 2019. Synthesis and machine learning for heterogeneous extraction. In Proceedings of the 40th ACM SIGPLAN Conference on Programming Language Design and Implementation (PLDI 2019). Association for Computing Machinery, New York, NY, USA, 301–315. https://doi.org/10.1145/3314221.3322485
