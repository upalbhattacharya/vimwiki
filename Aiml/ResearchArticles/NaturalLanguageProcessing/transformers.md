# Attention Is All You Need

* Dispenses with recurrence and convolution entirely and is based solely on attention mechanisms.

## Introduction

* The inherently sequential nature of recurrent networks precludes parallelization during training which becomes critical at longer sequence lengths.

* Attention mechanisms have become an integral part of compelling sequence modeling and transduction models by allowing modeling of dependencies without regard to their distance in the input or output sequences.

## Model Architecture

* The Transformer follows the general architecture of encoder-decoder models using:
	* stacked self-attention and,
	* point-wise fully conneted layers
  for both the encoder and the decoder.
  
### Encoder Overview

* It is composed of a stack of N (=6) identical layers.

* Each layer has **two sub-layers**:
	* **multi-head self-attention** mechanism
	* **fully connected feed-forward network**.

* Each sub-layer has a **residual connection** followed by **layer normalization**.

### Decoder Overview

* Also composed of a stack of N (=6) identical layers.

* Each layer has three sub-layers, two of which are the same as that of the encoder unit.
	* The third layer is a masked multi-head self attention mechanism which works with the output layers.
	* In this layer, the **subsequent positions are masked from the attention mechanism** so that **predictions are only made from the previous positions** as would be the case normally during testing and prediction. 

As with the encoder, each layer has a residual connection as well as a layer normalization.

### Attention

* An attention function (general) can be described as mapping a query and a set of -key value pairs to an output.

**Note:**
	In the context of the attention research article, the following are to be considered:
		
		* the **query** is the previous state $s_(i-1)$.
		* the **key** is the hidden state $h_j$.
		* the **value** is also the hidden state $h_j$.
		* the  **alignment model** $a(s_(i-1), h_j)$ gives a similarity model of how well aligned the 'query' is with the 'key'. 
		* the **similarity** is then the $alpha_ij$ terms i.e. the softmax of the exponential of the $a(s_(i-1), h_j)$ terms.
		* The output is then given by the weighted sum of the hidden layer values $h_j$ weighted by the similarity values $alpha_ij$.

#### Scaled Dot-Product Attention

* The input consists of queries and keys of dimension $d_k$ and values of dimension $d_v$.

* The attention for a set(matrix) of queries $Q$, keys $K$ and values $V$ is given by:
	
	
								  QK^T
	Attention(Q,K,V) = softmax( -------- ) V
							    sqrt(d_k)
								
* For very large values of $d_k$ the dot product attention grows large in magnitude, pushing the softmax function into regions where it has extremely small gradients. The scaling by $sqrt(d_k)$ helps counteract this effect.

#### Multi-Head Attention

* Instead of performing a single attention function, it is beneficial to linearly project the queries, keys and values multiple times with different learned linear projections.

* Each of the projected queries, keys and values are then used to perform attention on in parallel to get output values.

* Th several output values are concatenated and once again projected resulting in the final values.

* Multi-head attention allows the model to jointly attend to information from different representation subspaces at different positions. 
  
	MultiHead(Q, K, V) = Concat(head_1, ..., head_h) W^o
	
	head_i = Attention(QW_i^Q, KW_i^K, VW_i^V)
	
* 8 heads were used in the model.

#### Position-wise Feed-Forward Networks

* A two layer linear transformation with a ReLU activation is used in each layer of the encoder and decoder. A layer's feed-forward layer output is given by:
	
	$FFN(x) = max(0, xW_1 + b_1)W_2 + b_2$
	
#### Positional Encoding

* As the model uses no recurrence or convolution, relevant position information needs to be injected to the tokens of the sequence. 
  
* The positional encoding have the same dimensionality as the word embedding and is added based on a formula:
	
	PE(pos, 2i) = sin(pos/10000^(2i/d))
	PE(pos, 2i+1) = cos(pos/10000^(2i/d))
	
	where pos is the position and i is the dimension.
	
### Optimizer and Regularization

* Adam optimizer is used for training.
* Dropout is applied to the output of each-sublayer before it is added to the sub-layer input and normalized. Dropout is applied to the sums of the embeddings and positional encodings in both the encoder and decoder stacks with a rate of 0.1.
	
