
## 1. Kernel methods learn by comparing pairs of data points using particular similarity measures—kernels.

Consider a non-empty set of data points $X$, such as $\mathbb{R}^d$ or a finite set of graphs, and let $k: X \times X \to \mathbb{R}$ be a function. Then, $k$ is a kernel on $X$ if there is a Hibert space $H_k$ and a feature map $\phi$ : $X \to H_k$ such that $k(x,y)= \langle \phi(x), \phi(y) \rangle$ for $x,y\in X$, where $\langle .,. \rangle$ denotes the inner project of $H_k$.

*  $k: X \times X \to \mathbb{R}$
*  $H_k$ : Hilbert space
*  Feature map $\Phi : X \to H_k$ such that $k(x,y)= \langle \phi(x), \phi(y) \rangle$ for $x,y\in X$, where $k(x,y)= \langle \phi(x), \phi(y) \rangle$ for $x,y\in X$

An important concept in kernel methods is the $Gram$ $matrix$ $K$, defined with respect to a finite set of data points $x_1,...,x_m \in X$. The Gram matrix of a kernel $k$ has elements $K_{ij}$, for $i,j \in {0,...,m}$ equal to the kernel value between pairs of data points, i.e., $K_{ij}= k(x_i, x_j)$. If the Gram matrix of $k$ is positive semidefinite for every possible set of data points.

In this work, we restrict our attention to classification of objects in a non-empty set of graphs $\mathbb{G}$. In this setting, a kernel $k:\mathbb{G}\times\mathbb{G}\to\mathbb{R}$ is called a graph kernel. Like kernels on vector spaces, graph kernels can be calculated either $explicitly$ (by computing $\phi$) and $implicitly$ (by computing only $k$). Traditionally, learning with implicit kernel representations means that the value of the chosen kernel applied to every pair of graphs in the training set must be computed and stored. 

## 2. Design Paradigms For Kernels on Structured Data

The structure of a graph is invariant to permutations of its representation—the ordering by which vertices and edges are enumerated does not change the structure—and vector distances between, e.g., adjacency matrices are typically uninformative.

As mentioned previously, two graphs with identical structure (irrespective of representation) are called isomorphic, a concept that could in principle be used for learning.

However, not only is there no known polynomial-time algorithm for testing graph isomorphism but isomorphism is also typically too strict for learning—it is akin to learning with the equality operator. In practice, it is often desirable to have smoother metrics of comparison in order to gain generalizable knowledge from the comparison of graphs. Haussler’s Convolution Framework [18] is to decompose these two structures into substructures, e.g., vertices or subgraphs, and then evaluate a kernel between each pair of such substructures.


***Definition 2.1*** (Convolution Kernel). Let $R = R_1\times \dots \times R_d$ denote a space of components such as that a composite object $X \in \mathcal{X}$ decomposes into elements of $R$. Further, let $R: \mathcal{R} \to \mathcal{X}$ denote the mapping from components to objects, such that $R(x) = X$ if and only if the components $x\in \mathcal{R}$ make up the object $X \in \mathcal{X}$, and let $R^{-1}(X) = {x \in \mathcal{R}: R(X) = X}$. Then, the $R-convolution$ $kernel$ is

$k_{CV}(X,Y) = \sum_{x\in R^{-1}(X)} \sum_{y\in R^{-1}(Y)} \prod_{i=1}^d k_i(x_i,y_i)$ 

where $k_i$ is a kernel on $R_i$ for $i$ in $\{1,...,d\}$.


## 3. Graph Kernels

The subsequent subsections deal with $assignment$ and $matching-based$ kernels, and kernels based on the extraction of subgraph patterns, respectively.


## 3.1. Neighborhood Aggregation Approaches

One of the dominating paradigms in the design of graph kernels is representation and comparison of $local$ structure. Two vertices are considered similar if they have identical labels—even more so if their neighborhoods are labeled similarly. Expanding on this notion, two graphs are considered similar if they are composed of vertices with similar neighborhoods, i.e., that they have similar local structure.

Neighborhood aggregation approaches work by assigning an attribute to each vertex based on a summary of the local structure around them. Shervashidze et al. [35] introduced a highly influential class of neighborhood aggregation kernels for graphs with discrete labels based on the 1-dimensional Weisfeiler-Lehman (1- WL) or color refinement algorithm—a well-known heuristic for the graph isomorphism problem

* $Weisfeiler-Lehman (1-WL)$ algorithm ($color$ $refinement$ algorithm


## 3.2 Assignment- and Matching-based Approaches

A common approach to comparing two composite or structured objects is to identify the best possible matching of the components making up the two objects. 

In the OA kernel, each vertex is endowed with a representation (e.g., a label) that is compared using a base kernel.

Then, a similarity value for a pair of graphs is computed based on a mapping between their vertices such that the total similarity between the matched vertices with respect to a base kernel is maximized.


## 3.3 Subgraph Patterns


Graphlet kernels count the isomorphism types of all induced (possibly disconnected). 

The time required to compute the graphlet kernel scales exponentially with the size of the considered graphlets.

To remedy this, Shervashidze et al. [26] proposed two algorithms for speeding up the computation time of the feature map for 𝑘 in {3, 4}. In particular, it is common to restrict the kernel to connected graphlets (isomorphism types).

However, the concept (but not all speed-up tricks) can be extended to labeled graphs by using labeled isomorphism types as features, see, e.g., [80].

Mapping (sub)graphs to their isomorphism type is known as graph canonization problem, for which no polynomial time algorithm is known [17]. However, this is not a severe restriction for small graphs such as graphlets and, in addition, well-engineered algorithms solving most practical instances in a short time exist [81]. Horváth et al. [27] proposed a kernel which decomposes graphs into cycles and tree patterns, for which the canonization problem can be solved in polynomial time and simple practical algorithms for this are known.


