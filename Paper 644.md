## Title: Combining Formal and Informal Information in Bayesian Program Analysis via Soft Evidences


We propose a neural-symbolic style of program analysis that systematically incorporates informal information in a Datalog program analysis. 

We apply a neural network to judge how likely an analysis fact holds based on informal information such as naming of program elements, comments, coding styles, and others. 

This information is encoded as a soft evidence in the probabilistic analysis, which is a "noisy sensor" of the fact.

With this information, the probabilistic analysis produce a better ranking of the alarms.

We have demonstrated the effectiveness of our approach by improving a pointer analysis based on variable names on eight Java benchmarks, and a taint analysis that consider inter-component communication on eight Android applications.




### Problem

Informal information (variable name, comment, coding style) is largely ignored in the literature. While this information does not give any formal guarantee, it can serve as good hints to estimate how likely a program fact holds.

To use the informal informations, we need to address several challenges.

+ How do we incorporate informal information without breaking the soundness of existing analysis?

+ How do we automatically incorporate such information in existing program analyses?

+ How to extract informal information that is useful to the current analysis?



To address these challenges, we take a neural-symbolic approach that is expressed in the form of probabilistic logic programs together with neural networks.

+ By attaching probabilities to logical analysis rules, it allows us to convert a conventional logic-based program analysis into a Bayesian model. By considering informal information, the probabilities will be updated. As a result, considering informal information will only update the ranking, but not break soundness. (exist)

+ By converting the analysis into a Bayesian model, we can easily extend the analysis to incorporate the informal information by extending the model. More concretely, we are going to use neural networks to estimate how likely a program fact derived by the analysis holds. (technical contribution)

+ 



