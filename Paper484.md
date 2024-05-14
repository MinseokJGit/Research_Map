
# Title: MEA$^2$: a Lightweight Field-Sensitive Escape Analysis with Points-to Calculation for Golang

Escape analysis plays a crucial role in garbage-collected languages.

However, Go employs a fast yet conservative excape analysis, which is field-insensitive and omits point-to-set calculation to expedite compilation.

In this paper, we propose MEA$^2$, an escape analysis framework atop GoLLVM (an LLVM-based Go compiler), which combines field sensitivity and points-to analysis. Moreover, a novel generic function summary representation is designed to facilitate fast inter-procedural analysis. 

The results show that,  compared to Go's escape analysis, MEA$^2$ can reduce heap allocation site by 7.9% on average (up to 35.5%). All this is achieved while keeping the time overhead of escape analysis within 1% of the compilation process.





## Problem

The current Go compiler implements a conservative escape analysis on the Abstract Syntax Tree (AST) to trade off for faster compilation. It treats any object as an indivisible memory unit, without distinguishing between objects and their fields (i.e., it is field-insensitive). It does not calculate the points-to set of variables or objects, resulting in overly conservative handling of indirect memory access operation like `*p = a`, causing unnecessary heap allocation for a.


## Solution

Our objective is to explore an escape analysis algorithm with moderately improved precision, ensuring minimal compile-time overhead while integrating field sensitivity and pointer analysis. Introducing field sensitivity and pointer analysis to Go's escape analysis is a challenging and unexplored frontier in current analysis, adapting these advancements to Go entails significant challenges due to the inherent differences between Java and Go.

MEA$^2$ introduces an abstraction called the *escape graph* to model program dataflow, with variables and their fields represented as nodes and data flows as edges. 

The escape graph is constructed through an on-the-fly process, and escape states of variables are computed based on paths in the escape graph. To achieve field sensitivity, we introduce types to nodes in the escape graph and use type compatibility constraints to model data flow accurately under field sensitivity. Additionally, MEA$^2$ employs reachability analysis in the escape graph to compute points-to sets as needed. We also propose a novel generic and configurable abstraction of function summaries, allowing MEA$^2$ to generate or load context-insensitive function summaries during inter-procedural analysis




