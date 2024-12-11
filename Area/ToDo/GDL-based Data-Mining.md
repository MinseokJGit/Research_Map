---
Problem: Data Mining
Impact: Impactful
State: ToDo
Target Conference: 
Main Author: []
Deadline:
---

## Preliminaries

### Definition 1. Graph
A graph is defined as $G = (V, E)$ with a vertex set $V$, also denoted as $V_G$ and an edge set $E \subseteq V \times V$, also denoted as $E_G$. The size of $G$ is defined as the number of edges, $|G|=|E_G|$, and the node degree is denoted as $deg_v$

### Definition 2. Subgraph Monomorphism (SM)
Given a subgraph $S$ and a target graph $G$, SM exists if there is a function $f: V_S \to V_G$, where $\forall (u,v)\in E_S$, $(f(u)), f(v)\notin E_G$. 

### Definition 3. Frequent and Infrequent Subgraph
Given a set of target graphs $\mathbb{G} = \{G_1,G_2,\dots,G_n\}$. A subgraph $S$ is a _frequent_ subgraph if SM exists between $S$ and at least $n*a$ graphs. Otherwise, $S$ is _infrequent_.

### Definition 4. Maximal Frequent Subgraph
Given a frequent subgraph $P$, if $E$ an edge $e_i$, s.t. $e_i\in P$, and $Q = P \cup e_i$ is infrequent, $P$ is termed a maximal frequent subgraph of $\mathbb{G}$. 

### Definition 5. The Margin Space
The Margin Space consists of all MFSs $P$ and their infrequent children $Q$ as defined above.

### Definition 6. The MFS Mining Problem
Given a set of graphs $\mathbb{G}$, the MFS mining problem is to identify all MFSs.

#### Practical MFS mining problems
Getting the full set of MFSs is generally not practical even for graphs with tens of edges. Therefore, in practice, an alternative goal is often adopted: within a limited time, an algorithm tries to identify as many MFSs as possible and also identify the MFSs of as large sizes as possible.




