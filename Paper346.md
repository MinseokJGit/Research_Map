# Title : Qantitative Weakest Hyper Pre: Unifying Correctness and Incorrectness Hyperproperties via Predicate Transformers


We present a novel weakest pre calculus for reasoning about quantitative hyperproperties over nondeterministic and probabilistic programs.





Problem:
Hoare Logic is a proof system for establishing partial correctness of programs, properties of individual executions that will always hold if the program terminates. However, certain properties (e.g., establishing that a system is secure via confidentiality, intergrity, or authenticity) cannot be expressed in terms of individual executions and are therefore beyond the scope of classical Hoare Logic. This is because attackers may compare several different traces to infer hidden secretes.


solution:
In this paper, we present a novel weakest pre calculus (whp) for reasoning about quantitative hyperproperties over programs with effects that cause the program execution to branch such as nondeterminism or probabilistic choice. 

We generalize existing work on quantitative weakest pre calculi by considering program termination from initial sets of states or initial probability distributions rather than single initial states. We thus obtain weakest preconditions for HHL and enable reasoning about so-called hyperquantities, which include expected values, but also more general quantities that were not supported before, employing hyperquantities evaluated in probability distributions.


Our calculus reveals novel dualities between forward and backward transformers, correctness and incorrectness, as well as nontermination and unreachability


Hyperproperties??




