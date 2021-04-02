# Legal Judgement Prediction via Topological Learning

## Abstract

* In real-world scenarios, legal judgement usually consists of multiple subtasks, such as the decisions of applicable law articles, charges, fines and the term of penalty.

* There exists topological dependencies between these subtasks.

* Proposed model formalizes the dependencies among subtasks as a Directed Acyclic Graph and proposes a topological multi-task learning framework

## Introduction

* Legal judgement prediction aims to predict the judgement results of legal cases according to case facts and descriptions.

* **Legal Judgement Prediction is confronted with two major challenges**:
	
	* **Multiple Subtasks in Legal Judgement**:
		
		* Legal judgement consists of complicated subclauses such as charges, term of penalty and fines.
		* The prediction of relevant articles can also be considered to be one of the fundamental subtasks which guides the prediction for other subtasks.
		
	* **Topological Dependencies between Subtasks**:
		
		* For human judges, there exists a strict order among the subtasks for legal judgement.
		
		* Simulation of judicial logic of human judges to model the topological dependencies among legal subtasks deeply influences the credibility and interpretability of judgement prediction and is a difficult task.

* To address these issues, TOP-JUDGE models the multiple subtasks in judgement prediction jointly under a novel multi-task learning framework.

* Models the topological dependencies among subtasks as DAGs.

* Given an encoded fact description, TOPJUDGE predicts tthe ouptuts of **all the subtasks following the topological order** and the output of specific subtask will be affected by all the subtasks it depends on.

* Since the model takes the explicit topological dependencies of LJP subtasks into consideration, it is flexible to handle other LJP subtasks.

* **Topological order of legal dependencies renders the model interpretable and reliable**.

* The article makes the following noteworthy contributions:
	* Explores and formalizes multiple subtasks of legal judgement under a joint learning framework. Formulates the dependencies among the subtasks of LJP as a DAG and add this prior knowledge to enhance judgement prediction.
	
	* Proposes a judgement prediction framework to unify multiple subtasks and make judgement predictions through topological learning.

## Method

### Problem Formulation

* A fact description of a case $x$ is given as a sequence of $n$ words as:
	
	$X = {x_1, x_2, ..., x_n}$
	
  where each word $x_i$ comes from a fixed vocabulary $W$.
  
* Based on the fact description $X$, the task of LJP $T$ aims to predict judgement results of applicable law articles, changes, term of penalty, fines and so on. $T$ is assumed to contain $|T|$ subtasks given as: $T = {t_1, ... t_|T|}$ each $t_i$ being a classification task.

### DAG Dependencies of Subtasks

* Assume that the dependencies among multiple subtasks of LJP form a DAG. The dependency set of a subtasks $t_i$ includes all subtasks $t_j$ whose predictions are required for the prediction for the subtask $t_i$.

### Neural Encoder for Fact Descriptions

* The word sequence input $X$ is encoded to get a fact description that can be fed to TOPJUDGE.

* Uses a CNN for generation of the fact description in a three-part system.
	
	* **Lookup**: Each word $x_$ is converted into a word embedding to get a vectorized representation of the textual description.
	
	* **Convolution**: A CNN with a kernel of size $h$ and $m$ filters is used to produce a feature map. Here, a kernel of size $h$ means taking the word vectors $x_i$ to $x_(i+h-1)$ of the word embeddings of the document.
	
	* **Pooling**: A per-dimension/filter max-pooling is applied to gain the final fact representation $d$.

### Judgement Predictor over DAG

* Based on the DAG assumption and ordered task list $T*$ is obtained.

* For each subtask $t_j$ the prediction of its judgement result is made based on the fact representation vector $d$ and the judgement results of its dependent tasks.

* For prediction, an LSTM cell is used to generate the outputs for each subtask in a three-step process:
	
	* **Cell initialization**: For a subtask $t_i$ with its given dependency set, the context and hidden states of the LSTM cell are initialized through a simple MLP with the hidden and context states of the dependency set subtasks as input. It can be given as:
	
		 	 	___		  	  	
		 h_j  =	\		W_ij  h_i + b_j
		 c_j    /__			  c_i 
				t in D_j
	
	* **Task-Specific Representation**: The fact representation $d$ and the initialized hidden and context weights are applied to an LSTM cell whose hidden states output is taken as the task-specific representation of the corresponding subtask.
	
	* **Prediction**: The hidden state output is passed through an MLP layer followed by a softmax to get the prediction.

* Cross-entropy across is taken as the loss function for a particular subtask.

### Training

* A weighted sum of the subtask crossentropy losses is used as the loss function, where each subtask loss is weighted by a weight factor.

## Results and Analysis

* The DAG used is:
	* $D_1 = null, D_2 = {t_1}, D_3 = {t_1, t_2}$
	* This is categorized as prediction of charges depends onf law articles and prediction of term of penalty depends on both law articles and charges.

### Ablation Studies

* Removal of any of the DAG connections decreases the overall scores of TOPJUDGE as well as the ability of correct prediction subtasks $t_2$ and $t_3$ that are logically related to the previous subtasks.

* Removal of cell initialization simulates loss of dependencies. Removal of Task-Specific Representation also reduces the ability. Therefore, the best performance is observed when using both.
