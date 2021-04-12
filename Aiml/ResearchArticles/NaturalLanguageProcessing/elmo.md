# Deep contextualized word representations

## Abstract

* Created deep contextualized word representations that models both:
	* complex characteristics of word use
	* how usage varies across linguistic contexts.

* The learned features are learned funtions of the internal states of a deep bidirectional language model using BILSTM pretrained on a large text corpus.

## Introduction

* ELMo word representations differ from traditional word embeddings in that **each token is assigned a representation that is a function of the entire input sentence**.

* ELMo representations are deep in that they are a learnt linear combination of the internal layers of the biLM.

* Combining the internal states in this manner allows for very rich word representations with:
	* **higher level LSTM states capturing context-dependant aspects of word meaning that can be used without modification to perform well on supervised tasks.
	* **lower-level states capturing model aspects on syntax that can be used for POS tagging.

## ELMo: Embeddings from Language Models

* The embeddings are computed on top of two-layer biLMS.

### Bidirectional language models

* Given a sequence on $N$ tokens(words) $(t_1, t_2, ..., t_N)$ a **forward language model** computes the probability of the sequence by modeling the probability of the token $t_k$ given the history:

							  N
							 ___
	p(t_1, t_2, ... , t_N) = | | p(t_k | t_1, t_2, ... t_k-1)
							 | |  
							 k=1
						
* This can be modelled with a context-independant token representation passed through $L$ layers of forward LSTMS.

At each position $k$, each LSTM layer outputs a context-dependent hidden output representation $h_k,j^F$ where $j = 1, ..., L$.

* In a similar manner, a **backward language model** predicts the previous token in a sequence given the future context:

							  N
							 ___
	p(t_1, t_2, ... , t_N) = | | p(t_k | t_(k+1), t_(k+2), ... t_N)
							 | |  
							 k=1
							 
 Similarly, at each position $k$, the backward LSTMS output hidden layer values given by $h_k,j^B$.
 
* A biLM combines both forward and backward LMs. The ELMo framework jointly maximizes the log likelihood of the forward and backward directions:

	 N
	___
	\	(log p(t_k | t_1, ... t_k-1) + log p(t_k | t_k+1, ... , t_N))
	/__
	k=1
	
### ELMo

* It is a task specific combination of the intermediate layer represenations in the biLM.

* For each token $t_k$ a L-layer biLM computes a set of $2L+1$ reprsentations:
	
	R_k = {x_k, h_k,j^F, h_k,j^B | j = 1, ... , L}
	    = {h_k,j | j = 0, ... L}
		
 where $h_k,0$ is the token layer and h_k,j is the biLSTM layer obtained by concatenating the respective forward and backward hidden layer representations.
 
* The representations $R$ for a token is collapsed into a single vector which can just be the top layer by ELMo to get the representation.

* More generally, the ELMo representation is given by a task specific weighting of all biLM layers:

							  L
							 ___	
	ELMo_k^task = gamma^task \	  s_j^task h_k,j
							 /__
							 j=0
							 
	where $s^task$ are softmax-normalized weights and the scalar parameter $gamma^task$ allows the task model to scale the entire ELMo vector.
	
* Considering that activations of each biLM layer have a different distribution, it may also help to apply layer normalization to each layer before weighting.
				  
	
