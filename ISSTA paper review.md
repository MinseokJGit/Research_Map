## Title: Enhancing IR-based Fault Localization using Large Language Models

## SBFL

Cons : necessitate both successful and unsuccessful executions, which introduce significant runtime overhead. ***Really?***

## IRFL
Information retrieval-based fault localization

IRFL calculates the similarity between bug reports and the potentially erroneous program.

In this context, bug reports function as queries, and their similarity to various software entities is gauged.

The subsequent rankings, informed by these similarity scores, illuminate the relevance of software entities to the specific bug report.

Researchers have exhibited increasing interest in IRFL, with numerous studies investigating diverse aspects to improve its effectiveness.


## Challenge:
difficulty of constructing effective queries for retrieval.

Existing methods have struggled to truly "understand" the content of bug reports. Their primary challenges has been the literal interpretation of the text without comprehending the underlying issues or concerns raised within the report.

LLMs have demonstrated a remarkable ability to understand and interpret natural language, capturing nuances and contexts that previous systems might overlook.