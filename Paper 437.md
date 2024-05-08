# Title : Partially Flow-Sensitive Points-to Analysis using Predicates


In this paper, we present a novel lightweight approach to partial flow sensitivity using value propagation graphs with predicate edges, encoding the relationship between a branching condition and the instructions within its branches.

Our predicated analysis tracks the flow of both primitives and objects using a Predicated Value Propagation Graph, whose nodes only propagate values if they are enabled by their predicates.



## Analysis Overview


Each reachable method is transformed into a _Predicate Value Propagation Graph (PVPG)_ that represents the flow of both primitive values and objects. 

## PVPG

node == methd????

_flow_ : nodes in PVPG connected by three types of edges

_use_ : edges for classical value propagation
_predicate_ : edges for signalling when a given flow become executable.
_observed_ : edges for representing other types of dependencies.



## Filter Flows for Partial Flow Sensitivity

1. the analysis executed on a base language in static single assignment, which maintaines flow sensitivity for local variables.

To incorporate this kind of reasoning into the analysis, the condition is split into two FilterFlow

![[Pasted image 20240419131450.png]]



## Control Flow Predicates for Partial Flow-Sensitivity