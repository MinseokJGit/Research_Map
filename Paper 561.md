# Title : Accelerating Enumeration-Based Program Synthesis by Optimizing Domain-Specific Languages



## Problem:

 design of DSLs for a synthesis task still remains a black art.


## AMaze

Accelerate enumeration-based program synthesizers by _optimizing a given DSL_ based on a set of training problems in the target domain.

+ Performance of a DSL is strongly related to the features of the smallest solution programs in this DSL for the training problems

## Idea:

We sample a few solution programs for the problems, and translate the programs between DSLs by lifting the DSL modifications to programs, so as to approximate the smallest solutions in different DSLs.

AMaze employs a _data-driven_ approach. Given a starting generic DSL, and a. set of synthesis tasks that are representative in a target task domain, AMaze searches a sequence of modifications on the starting DSL yielding an effective DSL for the domain. To design an effective algorithm for this purpose, we fact two challenges.


## Evaluation:

Our evaluation on two general enumeration-based synthesizers and two different synthesis domains shows that AMaze could effectively accelerate the enumeration-based synthesizers with speedups of 3.29X-18.79X, enabling them to solve 14-57 more tasks. In particular, with our acceleration, the evaluated general synthesizers now outperform the state-of-the-art synthesizers on these domains that rely on domain-specific knowledge.





# Challenge

1) **It is difficulat to design an algorithm to identify sutiable DSL:** The first challenge is applying typical search algorithms requires an efficient way to evaluate the quality of a DSL identified during the search. 

-> To address this challenge, our key observation is that, given a problem, the smallest solution program in a DSL strongly decides the efficiency of an enumeration-based synthesizer using the DSL.


2) **It is difficult to find small viable programs in a given DSL:** The second challenge brought by the above approach is how to efficiently get the smallest viable programs in a given DSL without solving the tasks.

->  Our solution to address this challenge is to sample a set of solutions for a problem, and use the solutions to approximate the smallest solution on a new DSL.

To avoid overfitting to the sampled solutions, we allow only the modifications to the DSL that are likely to be generalizable, and use a validation set to select the best DSL among a set of DSLs identified from the training set.


## Set up

The input of AMaze includes a starting DSL, a set of training tasks from a certain domain, and a given synthesizer. The output is a new DSL which incorporates a series of modifications on the original DSL.

Input : DSL X training tasks X synthesizer
