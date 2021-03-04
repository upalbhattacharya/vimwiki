# Neural Machine Translation by Jointly Learning to Align and Translate

## Abstract

* Models proposed for neural machine translation using encoder-decoder networks where the encoder encodes the input sentence into a fixed length vector which is then decoded by a decoder network to generate the translation.

* Use of a fixed-length vector is a bottleneck in improving the performance of the encoder-decoder architecture.

* Proposes to extend the idea by allowing a model to automatically soft-search for parts of an input sentence that are relevant to predicting a target word.

## Introduction

* The encoding of a variable length sentence into a fixed-length vector is problematic as it hinges on the encoder's ability to compress all necessary information into a fixed-length vector. This makes it difficult to deal with longer sentences, specially sentences that are longer than the sentences in the training set.

* In the proposed model, **each time the model generates a word, it soft-searches for a set of positions in a source sentence where the most relevant information is concentrated**. The model then predicts a **target word based on the context vectors associated with these source positions and all the previous generated target words**.

* It **does not attempt to encode a whole input sentence into a single fixed-length vector**.

* It encodes the input sentence into a **sequence of vectors and chooses a subset of these vectors adaptively while decoding the translation.**

## Learning to Align and Translate

* The new architecture consists of:
	* **a bidirectionall RNN as an encoder**.
	* **a decoder that emulates searching through a source sentence during decoding**.

### Decoder: General Description

* In the new model architecture, each conditional probability is defined as:
	
	$p(y_i|y_1, ..., y_(i-1), x) = g(y_(i-1), s_i, c_i)$
	
  where $s_i$ is an RNN hidden state for time $i$ computed by:
	
	$s_i = f(s_(i-1), y_(i-1), c_i)$
	
  $c_i$ is a context vector that depends on a sequence of annotations (hidden unit values of the 'encoder' network) $(h_1, ... , h_Tx)$ to which an encoder maps the input sentence.
  
* Each annotation $h_i$ contaings information about the whole input sequence with a strong focus on the parts surrounding the i-th word of the input sequence.

* The context vector $c_i$ is computed as a weighted sum of the annotations $h_i$:

			___
	$c_i =  \   alpha_ij h_j 
			/__
		 j=1 to Tx
		 
* The weight $alpha_ij$ of each annotatation is computed by:
	
					exp(e_ij)	
	alpha_ij = ------------------  (softmax)
				___
				\   exp(e_ik)
				/__
			 k = 1 to Tx
			 
   where $e_ij = a(s_(i-1), h_j)$ is an **alignment model** which scores **how well the inputs around position j and the output at position i match**.
   
* the alignment model is parameterized as a feedforward neural network which is **jointly trained with all the other components of the proposed system**.

* The weighted sum of all annotations can be interpreted as computing an expected annotation.

* $alpha_ij$ is the probability that the target word $y_i$ is aligned to or translated from a source word $x_j$. Then the i-th context vector $c_i$ is the expected annotation.

* $alpha_ij$ and $e_ij$ reflect the importance of the annotation $h_j$ with respect to the previous hidden state $s_(i-1)$ in deciding $s_i$ and generating $y_i$

### Encoder: Bidirectional RNN for Annotating Sequences

* The proposed scheme aims that the annotation of each word does not only summarize the preceeding words but also the following words. Thus, a bidirectional RNN is used.

* The forward hidden states $h^f_i$ and the backward hidden states $h^b_j$ are **concatenated** to obtain an annotation for each word $x_j$. In this way, the concatenated annotation $h_j$ contains the summaries of both the preceding words and the following words. 
  
* Due to the tendency of RNNs to better represent recent inputs, the annotation $h_j$ will be focused on the words around $x_j$.
