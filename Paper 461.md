

# System overview

In this  examples, we utilize `use(x)` statements in code snippets. Their purpose is to represent an arbitrary instruction resulting in a use dependency of x at a given program point.

## Analysis Overview

Each reachable method is transformed into a _Predicate Value Propagation Graph (PVPG)_ that presents the flow of both primitive values and objects.


method  -> PVPG 

Nodes in the PVPG are called flows and can be connected by three types of edges: use edges for classical value propagation.

PVPG

node : flow

edge 1 : predicate

edge 2 : use

 edge 3 : ovserved

The graphs of individual methods are connected into an interprocedural graph by linking the actual arguments with the formal parameters.


## Filter Flows for Partial Flow Sensitivity

While the analysis is flow insensitive in general, we maintain a certain degree of flow sensitivity using multiple approaches.

+ First, the analysis is executed on a base language in static single assignment (SSA).
+ Second, we model conditional branches involving type checks and primitive comparisons to increase the precision.

## Control Flow Predicates for Partial Flow-Sensitivity

Control flow predicates model the relationship between a condition in a branching instruction and node within its branches.


## Method Invocations as Predicates

If we can prove that a method never returns, we can conclude that all the statements following the method invocations are unreachable. 

## Merge Flows

When multiple branches join in the control flow graph, it is necessary to merge the incoming values in the PVPG.


## Abstraction for Primitive Values.

As shown in the motivating example, the analysis needs to track some primitive values such as boolean constants.



# Predicated Value Propagation Graph

First, we define the base language that serves as the input to our analysis.

Second, we define the lattice representing values propagated through a PVPG.

Third, we proceed to the formal definition of PVPG.


Lattice domain of PredPTA

Values are propagated through the PVPG.


## Flows and Edges in the PVPG

PVPG models the flow of both primitive values and types interprocedurally. 



## Value Propagation in the Predicated Value Propagation Graph

This section presents the core analysis algorithm based on the PVPG data structure.



