We sincerely appreciate the reviewers for their constructive comments. We will incorporate all the comments into the revised paper.

  

In particular, we will include a “project-aware” LLM-based approach as a baseline in our evaluation. We checked that the performance of the state-of-the-art LLM-based approach [6] can also be improved using PAFL, even when the existing method was made “project-aware” by prompting project-specific information of past buggy versions.

For instance, in the cppcheck project, the MFR score of the project-aware LLM-based technique was improved by 11%. The improvement by PAFL is attributed to PAFL’s ability to learn low-level, project-specific bug patterns, complementing the LLM-based approach, which typically focuses on high-level bug patterns; the combination of these strengths was beneficial. We will include a detailed discussion and empirical results in the revised paper.

  

The detailed responses to the reviewers' comments are as follows.

  

## Reviewer A

  

**[Q1] One aspect the paper is lacking, particularly in RQ1, is explaining how crosswords evolve during the training/evaluation process. Was there an increase in the number of bugs the tooling could find after a certain number of iterations, and did crosswords converge? Do you have information on this, or could otherwise include it in the discussion section?**

  

[A] We thank the reviewer for pointing out the important aspect we missed. As the reviewer guessed, PAFL accurately localized a larger number of bugs after a certain number of iterations. We also checked that the crosswords are also converged after a certain number of iterations. In the revised paper, we will add a discussion section to present and discuss the detailed empirical results.

  
  
  

**[Q2] Since the concept of a crossword is relatively interpretable, that could also give developers insights into where to search for a particular bug.**

  

[A] As the reviewer pointed out, the learned crosswords can provide valuable insights. For example, variables that are assigned high suspiciousness scores in the learned crosswords can help developers identify areas to focus on when searching for a particular bug. The revised paper will include examples of learned crosswords and provide a detailed discussion on this aspect.

  
  

**[Q3] There are many out of memory problems when it comes to the DL based approaches. Could the authors elaborate on what exactly is happening?**

  

[A] In DL-based approaches, memory requirements increase with the size of Test Loc (Table 1), which represents the number of lines covered by the test cases.

This is because these approaches need to process matrices whose sizes grow significantly with the number of covered lines.

For instance, when Test Loc exceeds 15k (e.g., in libxml2, cppcheck, exiv2, openssl, proj, yaml-cpp, and Youtube-dl), CNN-FL and RNN-FL were unable to localize the faults.

In the largest program openssl (Test Loc = 102k), MLP-FL failed its procedure.

We will clarify this in the revised paper.

  
  
  

**[Q4] It is also unclear what the selection procedure for DL-based approaches was? Why not test it against state-of-the-art LLM-based approaches?</b>**

  

[A] The three DL-based baselines are from an existing work [1], which also aims to improve the performance of baseline fault localizers. In the revised paper, we will include the state-of-the-art LLM-based approach as a baseline [6] and demonstrate how PAFL improves the performance of the baseline.

  
  

**[Q5] How many bugs/fault lines in total are we looking at? I think displaying the percentage relative to the absolute number of buggy statements would help interpret the results.**

  

[A] In the revised paper, we will include the absolute number of buggy statements.

  
  

**[Q6] What exactly is a crossword of 0? Does it just modify scores based on a single token? If that's the case, and since the work heavily relies on token embeddings, I wonder to what extent the authors considered using actual embedding techniques to represent crosswords (like word2 vec). That would prevent overfitting to variable names by using vector representations.**

  
  

[A] When updating the score of a statement, a crossword of 0 (i.e., (0,0,0,0) in Table 7) considers only the embedding (i.e., the set of tokens) of the target statement, without accounting for tokens in adjacent statements. As the reviewer pointed out, the performance of PAFL is highly dependent on the embedding methods. In this work, we adopted a simple and straightforward approach, representing each statement by the set of tokens it contains. However, employing a more advanced embedding method could enhance PAFL’s performance (e.g., prevent overfitting). Exploring or developing better embedding techniques is an interesting future work. We will discuss this in the revised paper.

  
  
  

## Reviewer B

  

**[Q1] Are the statements in section 2 supposed to be across versions? In Section 2, the authors detail a couple instances where similar faults appear again across projects and then make broad statements about the overall faults in the project. Intuitively, if a developer made a mistake in one version, they themselves would've learn from it.**

  

[A] Our statements in section 2 refer to patterns observed across versions (i.e., similar fault patterns have redundantly appeared across versions).

In cppcheck, for example, many faults are related to token pattern matching. The following (buggy) code is from Figure 3a (cppcheck#10).

  

```cpp

bool isTemporary(bool cpp, const Token* tok,...){

...

if (Token::Match(tok->previous(), ...)){

...}

return true;}

```

The above buggy code missed a token pattern matching case between the statement `if (Token::Match(tok->previous(), ...))}" and "return true;}`.

Similar faults (i.e., incorrect token pattern matching cases) have appeared throughout the versions of cppcheck.

For example in the last buggy version of cppcheck (cppcheck#30), a similar fault happened as follows:

  

```cpp

const Token * Variable::declEndToken() const

{

Token const * declEnd = typeStartToken();

while (declEnd && !Token::Match(declEnd, "[;,)={]")) {

if (declEnd->link() && Token::Match(declEnd,"(|[|<"))

declEnd = declEnd->link();

declEnd = declEnd->next();

}

```

The above buggy code has incorrect token pattern matching in the `if (declEnd->link() && Token::Match(declEnd,"(|[|<"))` statement. The string `"(|[|<"` is replaced as `"(|["` in the fixed version.

  

Though the two faults share a common fault pattern (i.e., unsuitable token pattern matching), their specific details–both in the bug and the fix–are quite different, which explains why developers repeatedly make mistakes across versions.

For example, the bug in cppcheck#10 involved a missed statement, whereas the bug in cppcheck#30 stemmed from an incorrect token pattern matching string. As a result, the experience of bug fixing cppcheck#10 may not directly aid in resolving cppcheck#30.

  
  
  
  
  
  
  
  

**[Q2] When comparing to other methods, namely the deep-learning based methods, it seems a bit unfair that they are not given project-specific information to learn from.**

  

[A] The three DL-based baselines are inappropriate to use the past buggy versions as training data because they are designed to train models using only the current target buggy version [3]. The artifacts of the three DL-based approaches train the model using only the current buggy version.

  

From the test case execution information (i.e., feature vector where each feature corresponds to a statement of the program and the feature values present whether the test case executes the statement or not) and results (pass or fail), the three DL-based approaches (i.e., CNN-FL, RNN-FL, and MLP-FL) learn a model that predicts whether a test case (described as a feature vector) passes or fails. Then the DL-based approaches figure out important features (i.e., statements) that make the model predict the test case fails. The importance of the features becomes the suspiciousness score of the statements. As previous versions have different statements (e.g., different dimension of feature vector), the DL-based approaches are inappropriate to use the project-specific information of past buggy versions to give suspiciousness scores of the statement of the current buggy version.

  
  

Because the three DL-based approaches are unable to use the project-specific information, we will include an LLM-based baseline [6] trained with the project-specific information as a baseline in the revised paper.

  
  
  

**[Q3] Other baselines that would be good to have that would not require training a model would be using LLMs and few-shot prompting them with examples of faults from past versions of the project.**

  
  

[A] We will include the state-of-the-art LLM-based baseline [6] trained by prompting LLMs with the project-specific faults of past versions in the revised paper.

  
  

**[Q4] The presentation at times was confusing. The fact that an instance in the Crossword language was called a crossword was clunky and confusing. It needed to be made much more clear that PAFL was not a deep-learning technique -- the authors used "learn" and referred to many other deep learning baselines with it, so it got a bit muddled.**

  
  

[A] In the revised paper, we will change the language or instance into another name (e.g., "Fault Pattern Language"), and we will also replace "learn" with another one (e.g., "synthesize") in our training procedure of PAFL to avoid confusion.

  
  

## Reviewer C

  

**[Q1] The source code is not available with the submission and will only be published if the paper is accepted.**

  

[A] We provided artifacts along with the submission (lines 126 and 1128 - 1132). The submitted artifact includes the source code of PAFL.

  
  
  

**[Q2] Can you explain why the pairs of fixes in Figures 2 and 3 are considered repetitive?**

  

[A] In Figure 3 (cppcheck#10 and cppcheck#13), the two faults are both related to token pattern matching in parsing C/C++ programs. The both bugs missed token pattern matching cases, and the fix added the missed cases.

In Figure 3a (cppcheck#10) below, the fault is related to a variable `tok` which is responsible for parsing the tokens in a given C++ code.

  

```cpp

bool isTemporary(bool cpp, const Token* tok,...){

...

if (Token::Match(tok->previous(), ...)){

...}

return true;}

```

The fixed version added a token pattern matching case to deal with the variable `tok`.

  

The fault in Figure 3b (cppcheck#18) below is also related to the variable `tok`.

```cpp

static bool isDeadTemporary(bool cpp, ...){

if (!isTemporary(cpp, tok, library))

return false;

if (expr && !precedes(...))

return false;

return true;}

```

In the above buggy code, the statement `if (expr && !precedes(...))` also missed a token pattern matching case related to the variable `tok`. The fixed version also includes additional token pattern matching cases to address this.

  
  

In Figure 2 (exiv2#1 and exiv2#5), the two faults missed a logic processing the size of input metadata; the fix added the missed logic.

In Figure 2a (exiv2#1) below, the buggy code missed a logic to deal with the variable `resrcLength` which is related to the size of the input matadata.

```cpp

...

uint32_t resrcLength = getULong(buf, bigEndian);

while (resrcLength > 0)

...

```

The fixed version added a logic to enforce the size of the input metadata (i.e., add `enforce(resourcesLength < io_->size(), Exiv2::kerCorruptedMetadata);` statement).

In Figure 2b (exiv2#5) below, the statement `if (subBox.length > io_->size() - io_->tell()){` also missed a logic.

```cpp

...

subBox.length = getLong(...);

subBox.type = getLong(..., bigEndian);

if (subBox.length > io_->size() - io_->tell()){

throw Error(kerCorruptedMetadata);}

```

The fixed version added the logic. The buggy statement is fixed as `if (subBox.length < sizeof(box) || subBox.length > io_->size() - io_->tell()) {`.

  
  
  

**[Q3] In Figure 2.a, the fix was adding a statement so there's no faulty statement in the buggy version, what fault pattern can be learned there? How could it result in the learned crossword in Figure 5.b?**

  

[A] If a fix adds a statement without modifying existing statements, the previous and after statements of the added statement are considered as the fault statements, following the convention in prior works [2]. We will clarify this in the revised paper.

  

For example, the crossword in Figure 5.b is learned from the statement "while (resrcLength > 0)" that includes the token ">" and the previous statement "uint32_t resrcLength = getULong(buf, bigEndian);" includes the token (variable) "bigEndian".

  
  
  

**[Q4] How did you identify the buggy versions and more importantly the bug fixing changes? This itself is a challenging problem. The fact that there are only 12 projects from two datasets satisfying the criteria of having at least seven buggy versions with reproducible bugs shows that the number of real-world projects where the technique is applicable could be limited.**

  
  

[A] We used existing benchmark suites BugsCpp [4] and BugsInPy [5] providing buggy and fix changes for evaluating fault localization techniques. In the benchmark suites, each buggy and fix version was collected from existing real-world projects by using commit messages (e.g., whether the commit messages include "fix").

  

As the reviewer pointed out, if there is lack of bug fixing data, PAFL is inapplicable. However, many real-world projects are being developed with version control systems such as Git; enough bug and fix data has usually been accumulated during their development. The data can also be collected by using commit messages like existing benchmark suites.

  
  

**[Q5] Can you explain why Aeneas did not improve FLs at all?**

  

[A] We checked that this was because Aeneas removed several fault statements from the candidates. Removing the fault statements from the candidates significantly degraded the performance of Aeneas as the fault statements had the lowest suspiciousness ranking.

The artifact of Aeneas has two hyper-parameters, $cp$ and $ep$.

$cp$ determines the ratio of statements to be removed, and ep affects which statements are removed. For example, if $cp$ is 0.9, 10% of the statements are removed from the candidates using the ep value. We ran Aeneas with various configurations, $cp$ $\in$ {0.95,0.9,0.85,0.8} and $ep$ $\in$ {0.8,0.75,0.7}. We would like to note that Table 6 presents the performance of the best configuration in the 12 possible configurations (lines 967 - 974 and lines 981 - 988).

  

**[Q6] It's really hard to understand and follow the technical description of the paper. The main reason is that it jumps into the algorithms and tries to explain how they work without rationale/intuitions and high-level ideas. Having running examples would also help.**

  

[A] In the revised paper, we will include a section that that illustrates how our technique works with a running example before delving into the technical details.

  

**[Q7] Several concepts/terminologies are unclear or confusing. For example, I am not sure what is a fault in a program? Is it a statement or a token? How does the technique handle composite statements when converting a program into a sequence of statements?**

  

[A] In our paper, faults represent statements (e.g., program lines) that make the program fail test cases, and fixing the statements makes the program pass the test cases. If a composite statement is placed in the same line $l$, the composite statement is considered as a single statement (i.e., sequence of tokens at line $l$). We will clarify this in the revised paper.

  

**[Q8] It looks like the approach learns only one fault pattern for each buggy version. Why does each pattern have only one token when a fix of a buggy version might change multiple statements containing multiple tokens?**

  

[A] A fault pattern (i.e., crossword) has multiple tokens (not one token). For example, the crossword in Figure 5.b has at least two tokens (i.e., 'bigEndian' and '>'). The actually learned has more than twenty tokens, but we omitted them for simplicity. The example crossword in Figure 6.d has four tokens ('x', 'if', 'y', 'return', 'y').

  
  
  

**[Q9] In procedure PAFL in Algorithm 1, LearnFaultPatterns tries to optimize the enhanced FL which is only created in the next step. It's also not clear how the score of each token in the crosswords are updated through the iterations.**

  

[A] When localizing faults in $i$'th buggy version of a project, PAFL learns a set of fault patterns from the 1 ~ $i$-1'th buggy versions by calling LearnFaultPatterns. The score of each token in the crosswords is updated in the LearnFaultPatterns procedure described in Algorithm 2 (lines 540 - 555). The score of each token is updated in the iterations at lines 5 - 9 of Algorithm 2 (LearnFaultPatterns). The scores are updated to show the best performance in the given training data (i.e., the 1 ~ $i$-1'th buggy versions).

  
  

**[Q10] Several formulae were used without rationale why, e.g., the formula to adjust the suspiciousness score of the baseline in Section 3.2 and the formula to refine nodes in Section 3.4.**

  

[A] Formulas for adjusting the suspiciousness score is a design choice. We used one of the most simple and straightforward ones. We will clarify and discuss this in the revised paper.

  

**[Q11] The modeling of the faulty statement with directly adjacent statements is too simple to capture similar code snippets. Except for identical code snippets, semantically similar code snippets might not contain contiguous similar statements but interleaved which should be captured with data flows. It raises the question of how much it would work in practice.**

  

[A] PAFL may not be effective if buggy program points are syntactically different but semantically similar. However, our observation (e.g., Figure 2 and 3) shows that the faults are syntactically similar in many cases. PAFL improved the performance of the baselines mainly because the faults are syntactically similar.

  
  
  

### References

  

[1] H. Xie, Y. Lei, M. Yan, Y. Yu, X. Xia and X. Mao, "A Universal Data Augmentation Approach for Fault Localization," 2022 IEEE/ACM 44th International Conference on Software Engineering (ICSE), Pittsburgh, PA, USA, 2022, pp. 48-60, doi: 10.1145/3510003.3510136.

  
  

[2] S. Pearson et al., "Evaluating and Improving Fault Localization," 2017 IEEE/ACM 39th International Conference on Software Engineering (ICSE), Buenos Aires, Argentina, 2017, pp. 609-620, doi: 10.1109/ICSE.2017.62.

  

[3] Z. Zhang, Y. Lei, X. Mao and P. Li, "CNN-FL: An Effective Approach for Localizing Faults using Convolutional Neural Networks," 2019 IEEE 26th International Conference on Software Analysis, Evolution and Reengineering (SANER), Hangzhou, China, 2019, pp. 445-455, doi: 10.1109/SANER.2019.8668002.

  

[4] G. An, M. Kwon, K. Choi, J. Yi and S. Yoo, "BUGSC++: A Highly Usable Real World Defect Benchmark for C/C++," 2023 38th IEEE/ACM International Conference on Automated Software Engineering (ASE), Luxembourg, Luxembourg, 2023, pp. 2034-2037, doi: 10.1109/ASE56229.2023.00208.

  

[5] Ratnadira Widyasari, Sheng Qin Sim, Camellia Lok, Haodi Qi, Jack Phan, Qijin Tay, Constance Tan, Fiona Wee, Jodie Ethelda Tan, Yuheng Yieh, Brian Goh, Ferdian Thung, Hong Jin Kang, Thong Hoang, David Lo, and Eng Lieh Ouh. 2020. BugsInPy: a database of existing bugs in Python programs to enable controlled testing and debugging studies. In Proceedings of the 28th ACM Joint Meeting on European Software Engineering Conference and Symposium on the Foundations of Software Engineering (Virtual Event, USA) (ESEC/FSE 2020).

  

[6] Aidan Z. H. Yang, Claire Le Goues, Ruben Martins, and Vincent Hellendoorn. 2024. Large Language Models for Test-Free Fault Localization. In Proceedings of the IEEE/ACM 46th International Conference on Software Engineering (ICSE '24). Association for Computing Machinery, New York, NY, USA, Article 17, 1–12. https://doi.org/10.1145/3597503.3623342