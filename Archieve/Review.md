I appreciate the core idea of the paper that leveraging informal information can significantly improve the performance of program analysis.
The example in Figure 1 effectively illustrates the paper’s motivation. 
I have also observed that variable names in my own programs can provide useful hints.
The proposed approach of incorporating informal information through Bayesian analysis and neural networks is interesting.
Additionally, the paper demonstrates the generality of the approach, showing that it can also integrate dynamic information.
I have five questions listed below:

[1] The experiments use variable names and string constants as informal information in pointer and taint analyses, respectively. How could comments be incorporated as informal information in the proposed approach? Is the integration straightforward? Providing an example would help clarify this for readers. Additionally, how can other informal information, such as file names, be used? Can you offer guidance on incorporating arbitrary informal information? 

[2] There is no Data Availability Statement. Do you plan to make the artifacts accessible to readers?

[3] The performance of the approach might be ineffective if obfuscation techniques are applied to the programs, as they will alter variable names.
Please discuss this as a limitation of the proposed approach.

[4] The captions for Table 2 (a), (b), and (c) are confusing. Please add spacing between the captions for clarity.

[5] How long did the offline procedure (including the training of neural network models and probabilities for the Bayesian analysis rules) take?



---

### After Rebuttal

Thank you for the response. My major concern was about the artifact, and the response clearly addressed the concern. I have updated my score accordingly.



I updated my score as 'accept' and also updated my review comment. I recommend acceptacne, as my other concerns are minor.


