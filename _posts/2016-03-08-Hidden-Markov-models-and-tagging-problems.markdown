---
layout: post
title:  "Hidden markov models and tagging (sequence labeling) problems"
date: 2016-03-08
---

Estimating the Parameters of a Trigram HMM
We will assume that we have access to some training data. The training data consists   of a set of examples where each example is a sentence x1 . . . xn paired with a
tag sequence y1 . . . yn. Given this data, how do we estimate the parameters of the  
model? We will see that there is a simple and very intuitive answer to this question.  
Define c(u, v, s) to be the number of times the sequence of three states (u, v, s)  
is seen in training data: for example, c(V, D, N) would be the number of times the  
sequence of three tags V, D, N is seen in the training corpus. Similarly, define  
c(u, v) to be the number of times the tag bigram (u, v) is seen. Define c(s) to be  
the number of times that the state s is seen in the corpus. Finally, define c(s ❀ x)  
to be the number of times state s is seen paired sith observation x in the corpus: for  
example, c(N ❀ dog) would be the number of times the word dog is seen paired  
with the tag N.  
Given these definitions, the maximum-likelihood estimates are  

```javascript 
q(s|u, v) = c(u, v, s)|c(u, v)
```

and  
e(x|s) = c(s ❀ x) | c(s)

For example, we would have the estimates
