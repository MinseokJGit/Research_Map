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


## Approach of AMaze

AMaze considers two kinds of operations to modify a DSL: _composition_ and _deletion_. 

The composition operation adds new grammar rules by composing existing ones. For example, rule 𝐸 := 𝑥 + + can be added by composing 𝐸 := 𝐸 + + and 𝐸 := 𝑥.

The deletion operation removes production rules. 

_Preparing Stage_ 

AMaze first invokes the synthesizer with the starting DSL L0 to solve all the tasks.

1) We first apply a frequent pattern mining algorithm on the solution programs, and apply the composition operations only to introduce the mined frequent patterns. As a result, there are now six atomic modifications: one composition and five deletions. This in turn yields a search space of $2^6$ DSLs.

2) Amaze will use the solution programs to estimate the performance of a DSL, which we will show soon.

_Main Search Loop._ During the search, AMaze keeps track of the optimal DSL so far, and different solution programs in DSLs found in each iteration. In every iteration, AMaze tries to select an atomic modification on the current optimal DSL until it cannot improve the DSL anymore.

In the first iteration, AMaze applies atomic modifications on $L_0$ which yields 6 new DSLs. Next, we need to evaluate the performance of the synthesizer with these 6 new DSLs. A naive approach is to directly run the synthesizer with all DSLs on all tasks and compare their total running time. 

Since the synthesizer here is naive enumerator that enumrates the program from small to large, its running time for finding the solution program (i.e., the smallest viable program) is determined by the rank of the program in the enumeration list, which can be approximated by the number of programs that are not larger than it. Moreover, given a program or a set of programs, we can efficiently compute how many programs are not larger than it using a dynamic programming algorithm.

For a given new DSL, even its solution program is quite different from that of $L_0$, the time of finding $L_0$'s solution program is an upper bound of finding its solution program as long as $L_0$'s solution program is still viable in the new DSL.

We use the solution programs for the three tasks under $L_0$ to estimate the synthesizer's running time with the new DSLs and choose the DSL with the least predicted total running time.

AMaze predicts that overall $L_1$ which adds suffix is the best DSL among the new 6 DSLs and $L_0$. Concretely, the ranks of the solution programs of Task 1 and 2 increase while the rank of the solution program of Task 3 decreases. 

Note our prediction model is not perfect because the original solution program of Task 3 is actually not the smallest viable program in $L_1$.

To counter this issue and increase the accuracy of our prediction model, we track solution programs of more DSLs.


# Problem Definition

## 3.1 Preliminaries


