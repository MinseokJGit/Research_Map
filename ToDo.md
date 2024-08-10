

## PAFL
- [ ] Formalization tree




### Learn

In this representation, we assume the following functions:
\[
\successor, \predecessor, \parent, \child \in \Program \times \Stmt \to 2^\Stmt
\]
which respectively retrieve the successors, predecessors, parents, and children of a given statement


For example, the example program in Fig.~\ref{fig:formal:example_code} is represented by the sequence $\seq{s_1,s_2,s_3,s_4,s_5,s_6}$ with 

Let $\sem{-}$ be the instrumented program execution:
\[
\sem{P} : \Input \to \Output \times 2^{\Stmt}
\]