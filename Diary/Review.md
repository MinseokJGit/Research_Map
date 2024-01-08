

Overall Merit
-------------
Reject

Reviewer Expertise
------------------
Expert

Paper Summary
-------------
This paper presents a CFL-reachability formulation of kCFA for Java, integrating on-the-fly call graph construction. Existing $L_{FC}$-based CFL-reachability formulation loses precision because it relies on a separate call-graph construction algorithm. Section 2.2 provides a motivating example showing how the existing $L_{FC}$-based formulation loses precision even when a precise call-graph is given. To address the limitation, this paper presents $L_{DCR}$-based CFL-reachability formulation for constructing the call-graphs during the analysis (i.e., on-the-fly). To this end, the paper addresses three key challenges: (1) precisely passing the base variable to 'this' variable of the callee method, (2) establishing CFL-reachability path in a PAG (Pointer Assignment Graph), and (3) ensuring the parameter passing from actual parameter variables to formal parameter variables sharing the same context. To address the challenges, the paper presents new rules for building PAG (Figure 6).
The new rules construct a PAG with its newly introduced rules, [C-New], [C-Param], [C-Ret], and [C-VCall] for addressing the first and second challenges. The PAG construction rules also introduce new (boxed) symbols for describing inter-procedural value-flows for addressing the third challenge. After the construction of PAG, the $L_{DCR}$-based CFL-reachability formulation soundly handles parameter passing at virtual call sites sharing the same context (the third challenge is addressed). The paper also presents a selective context sensitivity technique $P3CTX$ (Section 4), using $L_{DCR}$-based pre-analysis. The evaluation results show the effectiveness of $P3CTX$ for kCFA. $P3CTX$-based kCFA (i.e., P-kCFA) preserved 100% precision of conventional kCFA, without selective context sensitivity, while other baselines (i.e., S-kCFA and Z-kCFA) loses precision. P-kCFA is also efficient. When k = 2, P-kCFA increases speed by about 3.2 times.




Comments for the Authors
------------------------
Overall, I appreciate the new $L_{DCR}$-based CFL-formalization of kCFA with built-in on-the-fly call-graph construction. The motivating examples are helpful for understanding the problem of the existing CFL-reachability formulation. However, I found practicality issues about the two contributions of the paper. The two contributions of the paper are (1) a new $L_{DCR}$-based CFL-reachability formulation of CFA and (2) $L_{DCR}$-enabled precision-preserving pre-analysis for kCFA with selective context sensitivity. However, I have concerns regarding the practicality of the two contributions as follows.



*Concern about the first contribution*
Why do we need the $L_{DCR}$-based CFL-reachability formulation instead of the inclusion-based formulation (i.e., Fig. 1) for kCFA? The problem of the previous $L_{FC}$-based CFL-reachability formulation that the paper aims to address is its imprecision as it relies on independent call-graph construction algorithms. However, this is not an issue in the inclusion-based formulation.
For instance, in the motivating example of Figure 3, the analysis results from the inclusion-based formulation (i.e., Table 1) are the same as those computed from the proposed $L_{DCR}$-based CFL-formalization. If I am misunderstanding, please let me know. What is the practical benefit of the $L_{DCR}$-based CFL-reachability formulation compared to the inclusion-based formulation? Applicability of demand-driven pointer analysis or context transformation (lines 36--37)? Is it straight-forward to apply the techniques (e.g., demand-driven pointer analysis or context transformation) in the new CFL-reachability formulation? 





*Concern about the second contribution*
Why do we need P-2CFA in practice instead of 2-object sensitivity with selective context sensitivity techniques such as [1, 2]? In the conventional k-context sensitive analysis (keeping the last k context elements), kCFA is far less precise than other contexts such as object sensitivity and type sensitivity [1]. The evaluation compares the performance of P-2CFA only with Z-2CFA (2CFA with Zipper [1]) and S-2CFA (2-CFA with Selectx). Why Z-2obj (2-object sensitivity with Zipper) is excluded as a baseline? Though not compared, Z-2obj (2-object sensitivity with Zipper) is likely to show a far better performance (in terms of precision) than Z-2CFA or P-2CFA. In the evaluation of [1], for example, Zipper with other contexts (Z-2obj, Z-2type, Z-2sobj) are more precise than 2CFA without selective context sensitivity. That is, Zipper with other contexts (e.g., Z-2obj, Z-2type, and Z-2sobj) will have better precision than P-2CFA (2CFA with the precision preserving selective context sensitivity). Then, why we need to use P-2CFA instead of Z-2Obj in practice?



I appreciate the new CFL-reachability formulation for kCFA, which holds theoretical value. However, the concerns I've raised about its practicality need to be addressed. If my understanding is incorrect (for example, if P-2CFA is more precise than Z-2Obj), please clarify it and provide experimental evidence to support practicality of P-2CFA.


## Minor

**On Section 2 Structure**:

The structure of Section 2 appears overly complex and may confuse readers. It currently includes multiple sub-sections, sub-sub-sections, and sub-sub-sub sections such as [2.1.1] Inclusion-based Formulation and [2.2.3.1] Using a Call Graph Construction in Advance. Simplifying this structure could enhance readability and help readers grasp the main points more effectively.

**On Section 4.2 (Evaluation)**:
I recommend separating Section 4.2 (Evaluation) into its own section (e.g., Section 5 Evaluation). This would allow a more focused discussion on the evaluation.


### References

[1] Yue Li, Tian Tan, Anders Møller, and Yannis Smaragdakis. 2020. A Principled Approach to Selective Context Sensitivity for Pointer Analysis. ACM Trans. Program. Lang. Syst. 42, 2, Article 10 (June 2020), 40 pages. https://doi.org/10.1145/3381915

[2] Jingbo Lu and Jingling Xue. 2019. Precision-preserving yet fast object-sensitive pointer analysis with partial context sensitivity. Proc. ACM Program. Lang. 3, OOPSLA, Article 148 (October 2019), 29 pages. https://doi.org/10.1145/3360574

