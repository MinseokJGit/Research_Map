# Title : Qantitative Weakest Hyper Pre: Unifying Correctness and Incorrectness Hyperproperties via Predicate Transformers


We present a novel weakest pre calculus for reasoning about quantitative hyperproperties over nondeterministic and probabilistic programs.





Problem:
Hoare Logic is a proof system for establishing partial correctness of programs, properties of individual executions that will always hold if the program terminates. However, certain properties (e.g., establishing that a system is secure via confidentiality, intergrity, or authenticity) cannot be expressed in terms of individual executions and are therefore beyond the scope of classical Hoare Logic. This is because attackers may compare several different traces to infer hidden secretes.


solution:
In this paper, we present a novel weakest pre calculus (whp) for reasoning about quantitative hyperproperties over programs with effects that cause the program execution to branch such as nondeterminism or probabilistic choice. 

We generalize existing work on quantitative weakest pre calculi by considering program termination from initial sets of states or initial probability distributions rather than single initial states. We thus obtain weakest preconditions for HHL and enable reasoning about so-called hyperquantities, which include expected values, but also more general quantities that were not supported before, employing hyperquantities evaluated in probability distributions.


Our calculus reveals novel dualities between forward and backward transformers, correctness and incorrectness, as well as nontermination and unreachability


Hyperproperties??


Noninterence: any two executions of a program with the same public inputs must have the same public outputs.
This guarantees that the program does not leak any secrete information to unprivileged observers.
As a demonstration, consider the following program, where the variable $l$ is publicly visible, but $h$ is secret.

$C_{ni} = assume ~h > 0; l := l + h$

+ Suppose we aim to prove $C_{ni}$ satisfies noninterference. 
 
Following the approach of logics such as Hyper Hoare Logic, one can define low($l$) to mean that the value of $l$ is equal in any pair of executions, and then attempt to establish the validity of |=$_{hh}$ {low($l$)} $C_{ni}$ {low($l$)}, meaning that if $C_{ni}$ is executed twice with the same initial $l$, then $l$ will also have the same value in both executions when (and if) the program finishes-hence, the initial values of $h$ cannot inference $l$.

HHL is sound and complete, but requires often require manual inventiveness.

HHL can disprove any of its triples, deriving either a positive or negative result (i.e., proving that a program is secure or not) requires one to know a priori which spec they wish to prove, or trying both.


In the case of the above example, our calculus leads us to a simple counterexample; if we have $l=0$ and $h=1$ in the first execution and $l=0$ and $h=2$ in the second execution, then clearly low($l$) holds, but the values of $l$ will be distinguishable at the end. This means that the program is insecure. 



## Classical Weakest Pre

![[Pasted image 20240507094622.png]]



Given a hyper postcondition, the weakest hyperprecondition anticipates for a given set of initial state, whether the set of states reachable from executing ~.

In other word, we are anticipating whether the strongest postcondition of $\phi$  satisfies the hypepostcondition

![[Pasted image 20240507095509.png]]

Reasoning about hyperproperties is strictly more expressive at it relates multiple executions. We showcase this in the following examples:


![[Pasted image 20240507095645.png]]



# Section 3 Syntax and Semantics

We introduce a language of commands _wReg_, which encompasses nondeterministic imperative constructs similar to those found in the Guarded Command Language. Furthermore, we adopt the weighting assertion as in cite, which enables representation of general weights over states. This includes reasoning of expected values over probability distributions, as studied in ~.


![[Pasted image 20240507104234.png]]

![[Pasted image 20240507104256.png]]



# Section 4 Quantitative weakest hyper pre

We define a novel quantitative strongest post transformer for wReg.