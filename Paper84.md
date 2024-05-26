## Title GenC2Rust: Towards Generating Generic Rust Code from C


The cost to rewrite this C code into Rust is likely to be prohibitive for many cases.

There is a mismatch between C abstraction and idiomatic Rust abstraction.


## Problem
Existing tools largely handle C pointers by generating raw pointers, negating much of the benefit of the Rust language. One many cause for generating raw pointers is the pervasive use of void pointers in C. In many cases, these void pointers could be translated into Rust generics. 

We conducted an evaluation of GenC2Rust across 42 C programs that vary in size and span multiple domains. Additionally, we offer a detailed analysis of the constraints encountered in the retyping of generic pointers.



### Intro

Rust’s static type system can in many cases verify the absence of memory safety errors and data races.

In general, it is desirable to minimize the amount of code written in unsafe Rust, because Rust does not guarantee the safety of such code. 

A challenge with C2Rust tools is that they often generate unsafe Rust code. One of the major issues is that polymorphism in C is often expressed using void* due to limitations of the C type system. Rust has support for generics and C polymorphism could often be safely expressed using Rust generics.

Unfortunately existing C2Rust tool is unable to analyze void* polymorphism in C and translate it into Rust generics.

We present a new tool, GenC3Rust, for analyzing void * pointer-style poly morphism from C and mapping it to Rust generics. 


### Approach Overview

Consider code has to provide the correct type information when instantiating a generic structure.

Consider translating the linked-list implementation in Figure 1 from C into Rust generics.

The generation of generic data structures requires careful analysis for two reasons.

+ The client code has to provide the correct type information when instantiating a generic structure.
+ The relation between type parameters has to be expressed in the translated data structure implementation

To solve these two challenges, our approach is to compute constraints between pointers in the C source code to translate the implicit C polymorphism into generic Rust. 

The constraint capture whether a pair of pointers have the same type, and whether there is any information about the actual type.

Our analysis uses two abstractions: _type graphs_ abstract heap- or stack-allocated structures and introduce type variables to data structure instances and _equivalence groups_ capture constraints between type variables.



### Analysis Abstractions


#### Type graph

We represent objects and their relations using a type graph $G=\{N,E\}$

The nodes $N$ represent both structures and pointers in the program. To differentiate the two uses of nodes, we use $N_s$ to represent the set of nodes that represents structures, and $N_p$ 


+ $N = N_s \cup N_p$ 
+ $N_s$ : Structure nodes
+ $N_p$ : Pointer nodes


+ $E = E_r \cup E_f$
+ $E_r$ : link structure node $n_s\in N_s$ to its set of pointer nodes $n_p \in N_p$.
+ $E_f$ : link a pointer node to the structure node $n_s \in N_s$

Algorithm 1 : type graph construction.


#### Equivalence group

An equivalence group, a set of nodes denoted as $E=\{n_1,n_2,\dots,n_m\}$, stores information about constraints between generic type parameters.

Equivalence groups tell us that two different variables, fields, or arrays must have the same type.

*Partitioning*

(1) generate per function equivalence groups

(2) After the intraprocedural constraint are computed, it propagate the constraints from each callee to its call-sites.


### Evaluation

GenC2Rust achieves decent runtime performance. The runtime column shows the total time for GenC2Rust to analyze and rewrite the entire program in seconds. 

As GenC2Rust's goal is to retype polymorphic void * pointers to generic pointers, it limits its candidates to void * pointer variables as well as void * pointer fields in structures. This set*

+ Runtime : total time for GenC2Rust
+ Total : candidate void pointer variables and pointer field variables
+ 


![[Pasted image 20240526143932.png]]







