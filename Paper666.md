
## Title : The Incredible Shrinking Context... in a decompiler near you


### Context

Decompilation of binary has arisen as a highly-important application in the space of Ethereum VM (EVM) smart contracts. Major new decompilers appear nearly every year and attain popularity, for a multitude of reverse-engineering of tool-building purposes.


### Problem

Technically, the problem is fundamental: it consists of recovering high-level control flow from a highly-optimized continuation-pass-style (CPS) representation. Archeitecturally, decompilers can be built using either static analysis or symbolic execution techniques.

### Solution

We present a static-analysis-based decompiler that achieves drastic improvements relative to the state of the art, in all significant dimensions: scalability, completeness, precision. Key technique is _shrinking context sensitivity_. 

### Evaluation

We compare out tool, Shrinkr, to state-of-the-art decompilers, both static-analysis- and symbolic-execution-based. In a standard benchmarkset, Shrinkr scales to over 99.5% of contracts (compared to ~95%), covers (i.e., reaches and manages to decompile) 67% more code, and reduce key imprecision metric by over 65%.


## Intro

Decompiling smart contracts is in high demand for several applications: building automated analyses over a uniform representation; understanding competitive trading strategies by trading bots; and much more.

The problem of decompiling EVM bytecode has received significant attention and new entrants constantly vie for adoption.

The problem of EVM decomiplation, however, is challenging because of two reasons.
+ The EVM bytecode language is extremely low-level with respect to control flow, replacing all execution-control constructs with jump instructions to an address popped from the execution stack.
+ The compiler  performs aggressive optimization in several code locations.


The challenge of EVM decompilation, thus, is to derive a higher-level representation, including functions, calls, returns, and structured control flow, from EVM bytecode.



This paper present a static-analysis-based decompiler that significantly advances the state of the art, on all quality dimensions (precision, completeness, scalability).

Compared to Elipmoc, the leading static-analysis-based decompiler, Shrinkr, achieves much greater scalability (up to 99.7% on the set of contracts that the Elipmoc team itself has used for evaluation, compared to Elipmoc's 95%), while substantially improving precision and completeness-virtually nullifying imprecision or incompleteness for most metrics.

Compared to the modern, most-adopted symbolic execution based decompiler, Heimdall-rs, Rhrinkr exhibits a large advantage, decompiling up to 67% more binary statements.

The technical essence of Shrinkr lies in several improvements over past static-analysis-based approaches.

The improvements in the fundamental static model are due to a new kind of static _context_ kept inside the decompiler


## Background

Smart contracts are small programs stored on a persistent blockchain as part of its state.

The execution setting of smart contracts exhibits several intricacies, many of which are relevant to our discussion of bytecode analysis and decompilation. 

In the EVM, basic blocks are explicitly delineated, via _JUMPDEST_ and _JUMP/JUMPI_ instructions. The flow between blocks, however, is far from clear.

In addition, compiler version and settings greatly affect the produced runtime bytecode.

We define the problem of EVM bytecode decompilation (a.k.a. _binary lifting_) as the derivation of high-level control-flow constructs and program structure from EVM bytecode.


![[Pasted image 20240528101212.png]]

The example program contains two external functions that accept various parameters and perform fund-transfer operations. 

### Public Function Patterns

As public functions are not inherently supported by the EVM, high-level languages adhere to the contract Application Binary Interface (ABI) specification, which specifies how input and output data are to be encoded when interacting with a smart contract.


### Shrinking context-sensitivity

Decompiler abstractly simulates all possible executions of the decompiled program. The decompiler collapses the stack into a finite _context_ structure.

The essence of the context-sensitivity algorithm is to decide which elements of the execution stack to keep at every point of modification.

Different dynamic executions that have the same context will be treated the same, with the analysis computing all possible values of a variable, instead of just a single value.

+ Gigahorse: N-call-site sensitivity
+ Elipmoc: transactional context-sensitivity  : N latest likely private function calls or returns.

Our approach dubbed _shrinking context sensitivity_, retains the two-part approach with a key distinction: the private function context can _shrink_ (much more drastically than merely discarding the oldest element), disregarding the context elements that are related to a likely private call, after that call returns to the first continuation pushed by its caller.


![[Pasted image 20240528111705.png]]



The current analysis context for block _cur_ is \[*pub*: u, *pri*: p\]

**Merge**(\[**pub**: _u_, **pri**:_p_\],_cur_, _next_) gives the analysis context for basic block _next_ when the analysis finds an edge from basic block _cur_ to _next_ and the current analysis context for block _cur_ is \[**pub**: _u_, **pri**:_p_\].


In English the above rule states. Upon a block transition,
+ if a public function entry is found, enter it as the public part of the context; otherwise
+ if a likely private function entry is found, push the caller block in the private part of the context, simultaneously dropping the oldest block in the context. Do the same if the transition is a likely function return, but it cannot be matched with a call in the context. Matching is performed by checking the continuation that the earlier likely call has pushed against the one the return is transitioning to;
+ if the block transition is a likely function return and can be matched with a call in the context, then drop all top-most private context elements until reaching the matching call.

The intuition behind shrinking context sensitivity is deceivingly simple: static context abstracts away the dynamic execution stack of the EVM. It then stands to reason that when the dynamic execution returns from a function call, no record of the function entry should remain on the static context, much like in the dynamic stack


The analogy is not perfect, however. 

First, as discussed, call and return block transitions are far from certain.

Second, in the dynamic execution stack, it is not the caller block of a function that is kept during the call, but only the continuation.

Truncating the context enables much greater precision later, since the context depth is finite. Additionally, the truncation logic offers a natural self-healing mechanism for the analysis abstraction of execution context: even if some inference (i.e., determining that a block may be a call and should thus be kept in the context) tuns out to be noisy, it will likely be pruned when enclosing function returns.






![[Pasted image 20240528113311.png]]







At this stage, the analysis can only, at best, grossly over-approximate what might be the possible calls and returns. However, the naming reflects the intuition: we want shrinking context sensitivity to attempt to match function calls and returns, and hopefully achieve both precision and scalability even with this incomplete information.  


![[Pasted image 20240528140629.png]]

_PrivateCallAndContinuation_ and _PrivateReturn_ are only _likely_ true to their name. 

The analysis cannot know for sure when a control-flow transition corresponds to a high-level function call.

The naming reflects the intuition: we want shrinking context sensitivity to attempt to match function calls and returns, and hopefully acheve both precision and scalability even with this incomplete information.



## Control Flow Normalization Via Cloning

A second technique that enables precision in Shrnkr is the aggressive cloning of blocks that are locally determined to be used in inconsistent ways. 

### Cloning Need: Illustration

Solidity compiler will often try to reuse the same low-level blocks for different high-level purposes. 

The code snippet in Figure 3 exhibits such behavior with block 0x72 being used to perform two of the three implicit checked subtraction operations in line 10 in Figure 1.



![[Pasted image 20240528162239.png]]


![[Pasted image 20240528162301.png]]

For this example, the state-of-the-art Elipmoc decompiler will correctly identify that this block is used to perform two different function calls, attempting to summarize them as shown in the three-address-code snippet in Figure 6a.

The Elipmoc output cannot be expressed using standard linear IR control-flow constructs over the basic blocks shown. The identities of the arguments of the two function calls are not recoverable. 

