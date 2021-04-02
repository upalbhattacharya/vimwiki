# Learning to Predict Charges for Legal Judgement via Self-Attentive Capsule Network

## Abstract

* Devises a self-attentive dynamic routing which can:
	 
	* capture long-range dependency more directly than vanilla dynamic routing and also learn the high-level generalized features better.
	* learn high-level generalized features better.

## Introduction

* Most works neglect the serious imbalanced data distribution in charge prediction task.

* Capsule networks can automatically learn a hierarchy of feature detectors via dynamic routing:
	* Consists of a group of neurons which uses vectors to represent an object or its part.
	* The orientation of the vector encodes properties of the object while its length reflects its probablity of existence.
	* Capsule networks can be quite appealing for catching high-level generalized features.

## Model

### Problem Definition

* Given a textual fact description $X = {x_1, x_2, .. x_n}$ where each $x_i$ is a word and a target prediction $y$, the goal is for the model to predict the distribution of charges given the fact description $X$.

### Fact Encoder Layer

* Each word $x_i$ of the textual fact description is mapped to a $p$ dimensional word embedding ${e_i, e_2, ... , e_n}$.

* The word embeddings are fed into a Bi-LSTM model to generate the **low-level capsules** $u_i$ for document $i$.

### Self-Attentive Capsule Layer

* Given the low level capsules ${u_1, ... u_k}$, self attention is computed using the scaled linear alignment methodology to compute the context vectors(as in the article "Attention is all you need"). The high level capsule $v_j$ is just a weighted sum of the context vectors as:

			___		 ___
	v_j =	\	w_ij \	 alpha_ij u_j W_j
			/__		 /__
	
	v_j = squash(v_j)
	
					  ||v_j||^2			  v_j
	squash(v_j) =  ---------------- . ----------
					1 + ||v_j||^2		||v_j||
					
	where $alpha_ij$ are the softmax outputs of the scaled alignment model outputs or the attention weights and $W_j$ is a linear transformation applied to $u_j$.
	The weights $w_ij$ are given by:
	
	w_i = softmax(b_i)
					___		 ___	
	b_ij = b_ij + (	\	w_ij \	 alpha_ij u_j W_j ) v_j
					/__		 /__
					
### Charge Prediction Layer

* The high-level capsules are passed through a fully-connected layer followed by a softmax.

* The data imbalance is handled by utilizing a multi-class **Focal Loss*.

