




## Paper Summary



This paper aims to enhance the performance of static program analysis by incorporating informal information, such as variable names, string constants, and comments.
Informal information such as comments has been ignored in static program analysis, yet it can provide valuable hints.
For instance, while not definitive, two variables named `dog` and `dolphin` are unlikely to be aliases. 
To utilize such informal information, the paper presents three key ideas.
This approach allows the probability of alarms to be influenced by informal information without sacrificing soundness.
Second, an additional rule is introduced to integrate informal information into the analysis.
Third, a neural network-based approach is used to learn models that predict the likelihood of relations based on informal information.
The proposed technique consists of two parts: offline and online.
In the offline phase, the technique trains neural networks and adjusts the probabilities for the Bayesian analysis rules using a set of training programs
In the online phase, it applies logical analysis to an unknown program using the models and analysis rules trained in the offline phase.
Evaluation results demonstrate that this technique enhances the performance of pointer and taint analysis by leveraging informal information.
In pointer and taint analysis, variable names and string constants are used as informal information, respectively. 
Additionally, the paper shows that the technique can be generalized to incorporate dynamic information from program executions.




## Comments for the Authors

I appreciate the core idea of the paper that leveraging informal information can significantly improve the performance of program analysis.
The example in Figure 1 effectively illustrates the paper’s motivation. I have also observed that variable names in my own programs can provide useful hints.
The proposed approach of incorporating informal information through Bayesian analysis and neural networks is interesting.
Additionally, the paper demonstrates the generality of the approach, showing that it can also integrate dynamic information.
Overall, I find this paper to be solid. I have a few minor questions listed below:


[1] The experiments use variable names and string constants as informal information in pointer and taint analysis, respectively. How could comments be utilized as informal information in the proposed approach? Is this integration straightforward? Could you provide an example to help clarify this for readers?

[2] The performance of the approach might be ineffective if obfuscation techniques are applied to the programs, as they will alter variable names.
Please discuss this as a limitation of the proposed approach.

[3] The captions for Table 2 (a), (b), and (c) are confusing. Please add spacing between the captions for clarity.

[4] How long did the offline procedure (including the training of neural network models and  probabilities for the Bayesian analysis rules) take?


