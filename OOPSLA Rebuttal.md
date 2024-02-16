The reviewers were satisfied with the interesting problems and techniques that suggest how to combine two main streams in static analysis. However, the paper needs to be more _**accessible**_ for readers who are not familiar with this topic. Hence, the reviewers decided to give a "Conditional Accept" to the paper. Please address the issues pointed out by the reviewers


learnability -> generalization ability

analysis derivation = Done

Modeling strategy  



Generalization

In other words, we want to choose an abstraction that can produce good alarm rankings with given amounts of posterior information. 

We refer  to the process of updating probabilities  of  alarms  based  on posterior information as generalization,  and  the  ability to  produce  good alarm rankings with a  given  amount  of  posterior  information as the generalization  ability  of a  Bayesian program  analysis. In the scenario  of  interactive  alarm  resolution, the stronger  the  generalization ability  is, the more true alarms will be ranked  first  with the same amount of  user feedback, or the  less user  feedback  is  needed to  identify  all true  alarms. 


Learned

which  is implemented as  a union of several N-dimension cubes. 

Good

Q1 = 

This setup  follows recent  works  in Bayesian program  analysis. In practice, the user may use other termination conditions.  For example,  they may  decide  to  stop  after consecutive alarms are false.  In our experiment,  it  is usually that  a  small fraction  of  the true alarms can  only be discovered  after inspecting many  false alarms.



Q13 = 

Since manual  inspection  of  real bugs requires  a lotof manual e!ort,  it is  common practice  in program analysis studies  [Jeon et al. 2020; Li et al. 2018a; Mangal et al. 2015; Zhang et al. 2017]  to use  an accuratebut heavy analysisto obtain  ground  truthfor  interactionor comparison.  In addition to  this, our  approach can be  viewed as  a  lightweight  but e!ective  way  to approximate an accurate but  heavy analysis  [Zhang et al. 2013] 

- [x] Please provide intuitive explanations about the terms pointed out by the reviewers  "abstraction level", "learnability",  "analysis derivation", "modeling strategy", "generalization".
- [ ]  Please clarify that the actual implementation of Learned.
- [ ]  Please incorporate the response to **Q1** (i.e., The user may have to inspect many alarms) and **Q13** (i.e.  How is the detailed search on line 1071 performed?) of reviewer B in the paper.
- [ ]  Please try to reflect the response to **Q7** and **Q9** in the revised paper.
- [ ]  Please correct grammatical errors in the paper
