# Title : Scaling Abstraction Refinement for Program Analysis in Datalog Using Graph Neural Networks


## Problem

Existing works cast abstraction refinements as constraint-solving problems. Due to the complexity of these problems, they cannot be scaled to large programs or complex analyses.

While constraint solvers have made great strides in recent years, it is still hard to scale them to problems that are produced by analyzing large programs due to the complexities of constraint problems. 

Furthermore, CEGAR-based approach sometimes may refine abstractions unnecessarily, as the constraint problems only encode why the current abstraction does not work but cannot predict accurately how unseen abstractions would work.


## Idea

We propose an approach that applies graph neural networks to improve the scalability of CEGAR for Datalog-based program analyses. By directly constructing graphs from the Datalog solver's calculations, we can then use a neural network to score abstraction parameters from the information in those graphs. Then we reform the constraint problems such that the constraint solver ignores parameters with low scores.



## Challenges

(1) How dow we apply deep learning to a large range of analyses without requiring heavy engineering from analysis designers?

(2) How can we select abstraction refinements that are truly useful for improving analysis precision?

(3) In practice, we find that while learning-based approaches are effective in pruning parts of abstraction parameters that do not need to be refined, it is  hard for them to identify exactly the parts that need to be refined.


## Overview

![[Pasted image 20240430110820.png]]

The graph neural network filters abstraction parameters to prune the MaxSat problem, and the MaxSat solver determines the next abstraction to try for the static analyzer by solving the Maxsat problem.


**Learning** Our approach performs a backward iteration through the trace.


(1) CI(11: id2)

(2) CI(9:id1)

$A_1 = {CS(11:id2)}$ to $A_2 = {CS(11:id2), CS(9:id1)}$

Then, we look for other helpful parameters that are not refined. We achieve this goal by trying all other abstractions like ${CS(11:id2),p}$. Those parameters $p$ that eliminate $q$ will also be marked as helpful. 

After processing this refinement, we add($A_1$, $\{CS(9:id1),CS(10:id2)\}$) to our training dataset, which means that our neural network should classify $\{CS(9:id1),CS(10:id1)\}$ as helpful given abstraction $A_1$ as input.