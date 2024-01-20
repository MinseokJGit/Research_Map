Title : Finetuning Large Language Models with Stack Traces: A Fault Localization Case Study (Experience paper)


# Overall merit

2. Weak Reject

# Reviewer expertise

3. knowledgeable


# Paper Summary

This paper finetuned a large language model (LLM), _Santacoder_, for localizing faults from stack traces of crashes in SQLite.
%
For training, the paper synthesizes a dataset by mutating (i.e., artificially injecting faults into) the source code of SQLite.
%
The synthesis procedure performs replacing symbols or deleting lines.
%
After a mutant crashes, a stack trace is extracted and included into the dataset.
%
From the synthesized dataset, the technique learns mapping between the stack traces and functions containing mutations.
%
In the evaluation, the synthesized dataset is split into a 9:1 ratio for training (i.e., finetuning the LLM model) and testing.
%
The evaluation results show that the finetuned LLM model is significantly more accurate than a baseline.
%
For example, the proposed technique achieved an accuracy of 83.3%, while the baseline achieved an accuracy of 34.7%.

# Strengths

+ Applying a trendy technique (LLM) to an important problem (fault localization).
+ This paper automatically generated training datasets, which would be useful in any machine learning-based approach.

# Weaknesses

+ The evaluation seems unfair as the training and test sets are produced from the same dataset generator (they are generated from the same mutation principles).
+ The technique seems general but is applied to only one LLM model.
+ The application is limited to SQLite


# Comments for authors

For finetuning the LLM model, the paper proposed a dataset generation procedure, and I think the automatically generating buggy programs (Section 3) is useful as such datasets are useful in training machine learning models or evaluating fault localization techniques.
%
Figure 1 was also useful for understanding the automatic dataset generation process.
%
Overall, I appreciate the proposed finetuning technique, but I have several concerns about the paper. Please address the following concerns ([1], [2], [3]). 


[1] The evaluation seems unfair as the training and test sets are produced from the same bug injection procedure.
%
In the dataset generation process (Section 3), the technique repeatedly performs the same mutation procedure; the mutants are produced withe the same principles.
%
In the evaluation, the dataset, produced from the same procedure, is split into a 9:1 ratio for training and testing; the finetuned LLM, trained with the training set, could be overfitted to the mutation procedure.
%
This could be a main reason why the proposed technique shows better accuracy than the baseline, which seems unfair.
%
To ensure a fair comparison, the accuracy of the trained model and the baseline need to be evaluated using an independent test dataset.
%
If I am misunderstanding, please let me know.


[2] Why have the authors applied the technique to only one specific LLM model, _Santacoder_?
%
I think the technique can be used in other LLM models as other models can also be finetuned with the produced dataset.
%
Demonstrating the effectiveness of the proposed technique using various LLM models would make the paper more valuable.


[3] I think the baseline, which chooses the innermost frame in the stack trace, used in the evaluation is inappropriate.
%
The paper presents a finetuning technique for an LLM model (lines 111 - 113); I think a suitable baseline would be an LLM model _**without**_ the finetuning.
%
Clarifying the performance differences before and after finetuning would more clearly demonstrate the necessity of the proposed technique.