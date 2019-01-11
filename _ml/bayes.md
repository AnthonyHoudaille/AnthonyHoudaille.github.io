---
published: true
title: Classification - Naive Bayes
collection: ml
layout: single
author_profile: true
---

The Bayes Classifier is a binary classification algorithm. It is often introduced as one of the first algorithms to master in the field of machine learning. 

## Bayes Theory
Bayes Theory allows us to determine posterior probabilities from priors when presented with evidence. Bayes theory is a fundamental probability result. It states that, for two given event $$ A $$ and $$ B $$ :

$$ ℙ(A|B) = \frac {ℙ(B|A)P(A)} {P(B)} $$

$$ ℙ(B|A) $$ 

is the conditional probability of the event B knowing A. This is the fundamental theory that motivates the development of the Naive Bayes Classifier.

## Notation

In binary classification, we try to predict labels : $$ Y $$ is either $$ 1 $$ or $$ -1 $$ .
The datas we use is a matrix $$ X $$ of observations with $$ d $$ features.

Our aim is to define a function $$ g $$ such that $$ g(X) \in (-1,1) $$ . As usual, we can define a loss minimization problem where : 

$$ L(g) = P(g(X) ≠ Y) = E(I(Y≠g(X)) = E(I(-Yg(X)) > 0) $$

## Solution

We should bear in mind that the law of $$ (X,Y) $$ is unknown. The aim of the Bayes Classifier is to define a decision function such that the probability of an observation $$ x $$ being under or above it is exactly $$ 0.5 $$. Therefore, if the output of the Bayes Classifier is above $$ 0.5 $$, the observation will be classified as belonging to the class $$  1 $$ , whereas in the other case, it would be classified as belonging to the class $$ -1 $$. The decision function is actually defined as the line for which : 

$$ ℙ(Y = 1|X ) = 1/2 $$

How do we minimize the loss function then ?

$$ L(g) = E( [I(Y=1)I(g(X) = -1)] + [I(Y=-1)I(g(X) = 1)]|X ) $$

This means we have two ways to make a mistake :
- by classifying an observation belonging to the class $$ 1 $$ in the class $$ -1 $$ , 
- by classifying an observation belonging to the class $$ -1 $$ in the class $$ 1 $$ .

By Fubini, we can develop the above stated equation :

$$ L(g) = E[ (I(g(X) = -1) E(I(Y = 1|X) + (I(g(X) = 1) E(I(Y = -1|X) ] $$

The prior probability is defined as : 

$$ \{mu} (x) = ℙ(Y= 1|X ) $$ 

We can replace in the loss function :

$$ L(g) = E[ \{mu} (X) I( g(X) = - 1) + (1- \{mu} (X) ) I( g(X)=1 ) ] $$

Then, for a given $$ x $$ , the minimium of $$ g(x) $$ is reached at $$ g(x) = 1 $$ for

$$ \{mu} (X) ≥ 1- \{mu} (X) $$ 

which means : 

$$ \{mu} (X) $$ ≥ $$ 1/2 $$ 

Otherwise $$ g(x) = -1 $$ -

We can define a general expression for the global minimum :

$$ g_{opt}(X) = I( \{mu} (X) ≥ 1/2 ) - ( 1 - I ( \{mu} (X) ≥ 1/2 ) ) $$

$$ = 2 ( I( \{mu} (X) ≥ 1/2 ) ) - 1 $$

> **Conclusion** : The Bayes Classifier has a major limitation since we need to know the joint distribution of $$ (X,Y) $$ . It is much easier to estimate statistically the decision frontier. This is what we are going to cover in the next articles.
