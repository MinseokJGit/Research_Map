---
Problem: Graph Machine Learning
Impact: Impactful
State: In Progress
Target Conference: 
Main Author:
  - Seunghyun
Deadline:
---

# Problem


# Language




# Theme : GDL 기반 그래프 기계학습 방법의 Neural network 버전



# GDLNN : GDL + Neural Network

(1) Use GDL and synthesis algorithm to generate graph features.
(2) Train MLP (multi-layer-perceptron) with the feature
![[GDLNN.png]]
장점: Accurate, explainable 
단점: **uninterpretable**, ??


### Producing Correct Subgraph Explanation

Idea: keep refining subgraph as long as doing so does not change the feature vector

![[GDLNN_Explanation_ALgorithm.png]]

### Why the subgraph is a correct explanation?

모델 입장에서는 원래 그래프(original graph)와 제공된 서브그래프는 같은 그래프이다 (동형이다).

설명 생성 알고리즘이 사용해야 할 GDL feature들의 성질:
+ 원래 0이었던 feature는 임의의 subgraph에서 항상 0이다.
+ 1의 값을 가지는 feature보다 subgraph 크기가 작아지만 해당 feature의 값은 0으로 바뀐다.


***Contribution:***
+ A new GDL-based model using neural network
+ We designed an algorithm that produces correct explanation
+ Experimental results



ToDo:

1. Node classification


Feature가 