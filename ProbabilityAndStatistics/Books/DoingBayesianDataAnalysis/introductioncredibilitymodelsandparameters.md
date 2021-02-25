# Chapter 2
# Introduction: Credibility, Models, and Parameters

Bayesian data analysis has two foundational ideas:
* Bayesian inference is reallocation of credibility across possibilities: Assume certain credibility to all posibilities and, as more information is provided, reallocate these credibilities to suitable possibilities.
* Possibilities, over which one allocates credibility are, parameter values in meaningful mathematical models: 
  
## Bayesian Inference is reallocation of credibility across possibilities

Given a set of possibilities in an experiment, one may assume certain prior probability to each possibility. Then, as more information is obtained, these probabilities are reallocated across the possibilities in accordance with the information.

This may happen in the form of information reducing the probability of one possibility thereby increasing the probabilities of the other possibilities in a continuos fashion till one possibility becomes fairly more probable than the rest or, it may happen that information strongly supports one possibility thereby reducing the probabilities of the rest.

### Data are noisy and inferences are probabilistic

In reality, data have only probabilistic relations to their underlying causes. Scientific measurements suffer from randomness and inaccuracies. The techniques of data analysis are designed to infer underlying trends from noisy data. This is done by accruing more data and incrementally adjusting the probabilities of possible trends. In Bayesian analysis, the mathematics reveals exactly how much of credibility is to be reallocated in realisitc probabilistic situations.

In real world applications, the data are noisy indicators of the underlying generator. The practice is to hypothesize a range of possible underlying indicators, and from the data infer their relative credibilities. 

Bayesian analysis is the mathematics of re-allocating credibility in a logically coherent and precise way. The initial distribution of credibility reflects prior knowledge about the possibilities and can be quite vague. As new data are observed, credibility is reallocated such that possibilities that are consistent with the data garner more credibility while possibilities that are not consistent with the data lose credibility.

## Possibilities are parameter values in descriptive models

Data analysis begins with a family of candidate descriptions for the data. The descriptions are mathematical formulas that characterize the trends and spreads in the data. These mathematical models contain parameters that determine the exact shape of the mathematical form. The role of Bayesian inference is to compute the exact relative credibilities of candidate parameter values, while also taking into account their prior probabilities.

Two main requirements for a mathematical description of data:

* The mathematical form should be comprehensible with meaningful parameters.
* The mathematical form should be descriptively adequate i.e. it should fit the data and its trends.


