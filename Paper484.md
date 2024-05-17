
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




## Background and motivation

### Memory Management in Go

During compilation, Go utilizes escape analysis to automatically determine the appropriate memory allocation location for objects, whether on the heap or the stack.

Go imposes two critical invariants on memory allocation decisions for objects.

* *Inv1.* Pointers to stack objects cannot be stored in the heap.
* *Inv2.* Pointers to a stack object cannot outlive that objects.


### Escape Analysis in Go

The naive Go compiler employs a graph-based approach for escape analysis, modeling memory locations as nodes in a graph. Edges are established based on operations like assignment, dereferencing, and address-taking, representing the data flow of the program. By analyzing the AST in a single pass, an escape graph is constructed, and the shortest path on this graph is calculated to determine the escape status of objects. Those identified as having escaped are ultimately allocated on the heap.

The escape analysis in GoLLVM, implemented in gofrontend and conducted on the AST, is similar in design to the native Go compiler's analysis but has higher time and space costs.


## Design Overview

The target of MEA$^2$ is to identify the lifetimes of  objects. The main design goals are as follows:

* *G1.* High precision: MEA$^2$ will introduce field-sensitive and points-to analysis in the escape analysis algorithm design to improve precision.
* *G2.* Fast compilation: The algorithm needs to control the analysis overhead within a certain range, ideally less than 1% of the total compilation time.
* *G3.* Configurability: It will allow the configuration of algorithm features such as field sensitivity through options. 
* *G4.* Extendability: To facilitate field-sensitive and points-to analysis, MEA$^2$ is designed on LLVM IR, making it easier to apply to various platforms or extend to other languages.




![[Pasted image 20240514145224.png]]


We start with an initial phase, which includes *creating data structures for the escape graph, determining the loop depth of variables, and initializing variable types and escape states.* 

We then iteratively navigate through the CFG, building and refining the escape graph for precise analysis. Once the escape graph is constructed, we simplify the escape graph, making the subsequent steps more efficient.

The final step involves applying optimization for stack allocation based on the escape states, aiming to improve memory management efficiency.

## Escape State

We can define a simple escape state lattice based on the escape state of the variables:

$NoEscape < Escape$

The escape state of variable $v$ is denoted as $ES(v)$. Before analyzing a function, all global variables and objects that are too large or of indeterminate size are initially marked as $Escape$. All other object allocated within the function are marked as $NoEscape$. Throughout the analysis, we ensure that the escape state of variables adheres to the following constraints:

* $v_a \in PointsTo(v_p) \implies ES(v_p) \le ES(v_a)$
* $v_a \in PointsTo(V_p), loopdepth(v_p) < loopdepth(v_a) \implies ES(v_a) = Esape$


### Escape Graph

*Definition.* Escape Graph is a directed weighted graph $EG = (N,E)$, $N = N_r \cup N_o$, $E = E_w \cup E_f$ where

* $N_r$ is the set of *reference nodes* that represent top-level variables
* $N_o$ is the set of *object nodes* that represent address-taken variables $A$ in a program.
* $E_w$ is the set of *flow edges*. Each flow edge represents the assignment relationship between two variables $v_{src}$ and $v_{dst}$
* $E_F$ is the set of *field edges*. It indicates that $v_{src}$ is the field of $v_{dst}$, and the field offset is $x$.
 
![[Pasted image 20240514152630.png]]

obj has two fields with offsets of 0 and 16, denoted as obj.0 and obj.16, respectively; the address of obj is obtained by the top-level variable a (address = -1), and the variable a is obtained by the top-level variable b (copy = 0).


## Intra-procedural analysis

### Escape Graph Construction for Field Insensitivity.

#### Transfer functions.

MEA$^2$ employs an iterative dataflow analysis approach to traverse the given function's CFG to construct its Escape Graph. The construction is an on-the-fly forward process, EG is updated at each instruction $I$ according to the transfer function $F^I$:
$EG_{(I*)} = F$


