
## Title : The Incredible Shrinking Context... in a decompiler near you


### Context

Decompilation of binary has arisen as a highly-important application in the space of Ethereum VM (EVM) smart contracts. Major new decompilers appear nearly every year and attain popularity, for a multitude of reverse-engineering of tool-building purposes.


### Problem

Technically, the problem is fundamental: it consists of recovering high-level control flow from a highly-optimized continuation-pass-style (CPS) representation. Archeitecturally, decompilers can be built using either static analysis or symbolic execution techniques.

### Solution

We present a static-analysis-based decompiler that achieves drastic improvements relative to the state of the art, in all significant dimensions: scalability, completeness, precision. Key technique is _shrinking context sensitivity_. 

### Evaluation

We compare out tool, Shrinkr, to state-of-the-art decompilers, both static-analysis- and symbolic-execution-based. In a standard benchmarkset, Shrinkr scales to over 99.5% of contracts (compared to ~95%), covers (i.e., reaches and manages to decompile) 67% more code, and reduce key imprecision metric by over 65%.



