# Semantic Text Matching for Long-Form Documents

## Abstract

* Proposes a Siamese multi-depth attention-based hierarchial recurrent neural network that learns long-from semantics and enables long-form document based semantic text matching.

* In addition to word information, SMASH RNN uses the document structure to improve the representation of long-form documents.

* An attention-based hierarchial RNN derives a representation for each document structure level(paragraphs, sentences and words).

* For semantic text matching, a Siamese structure couples the representations of a pair of documents and infers a probabilistic score as theri similarity. 
  
## Introduction

* The difficulties of semantic matching are:
	* semantics of words and phrass can be ambiguous.
	 
	* when text is long, semantics of individual words, phrases and sentences can be buried in complex document structures.

* While some works use the attention mechanism to distill important words from sentences for semantic text matching, valuable information can still be diluted within a large number of sentences and paragraphs in long-form documents.

* Working with word-level knowledge disregards structural information like relations between sentences and paragraphs.

* The semantics of a document can be spread across a long narrative. Neither RNNs nor CNNs can naturally capture or follow semantic drifts like this.

* Sequential processing of documents by RNNs can lead to confusing document representations by disregarding structural semantics like sentences and paragraphs.

* **Since natural language documents follow a general hierarchial structure to faciliate understanding, it is vital to utilize these structures in order to train machine learning models for the purpose of capturing the semantics of long-form documents.**

* In the proposed method, each tower of the two towers of the Siamese network for document semantic matching is a **multi-depth attention-based hierarchial RNN(MASH RNN)**

* MASH RNN can derive comprehensive document representations from multiple levels of document structure. Word, sentence and paragraph level knowledge of a document can be derived by three attention-based hierarchial RNNS with different depths. The concatenation of these three levels of document representation provides a comprehensive document representation accounting for all levels of semantic information.

* Combining the MASH RNN document representations of the source and target documents, SMASH RNN estimates the semantic matching scores between their representations.

## Semantic Text Matching for Long-form Documents with SMASH RNN

### Problem Statement

* Defines the hierarchy for each document as:
	* paragraphs
	* sentences
	* words

* Given a source document $d_s$ and a set of candidate documents $D_c$, the goal is to estimate the semantic similarity bertween the source document and every candidate document.

### MASH RNN for Document Representation

* Using only one level of information can cause loss of the information encoded in the other levels. 
  
* The computation of encoders in MASH RNN follows a bottom-up principle with bidirectional RNNS with attention.

* For the paragraph-sentence-word architecture, each word is first embedded with an embedding layer.

* With the word embeddings, a Bi-RNN (LSTM/GRU) provides the hidden representations for words in a sentence.

* Since each word can have unequal importance in a sentence, an attention mechanism with the hidden word representation is used to get the sentence representation.

* With the sentence representations, a Bi-RNN is applied to get the hidden representations of the sentences in a paragraph. As with words to sentences, the hidden representations of the sentences are passed through an attention layer to give the paragraph representations.

* These paragraph representations are passed through yet another Bi-RNN to get the hidden paragraph representations. Attention applied to these hidden representation provides the proper importance of each paragraph in the overall document to give the paragraph level encoding of the document and the paragraph level representation $dP$.

* In the same way that the paragraph representations are passed through a separate attention mechanism to obtain the paragraph level representaion of the document as $dP$, the word and sentence level document representations $dW$ and $dS$ are obtained by passing their representations through different attention layers.

* The concatenation of $dP, dS$ and $dW$ gives the overall document representation.
  
### SMASH RNN for Semantic Text Matching

* The Siamese structure associated with two idential sub-networks is known to be effective in measuring the affinity between the representation of two documents modeled in the same hidden space.

* For semantic text matching of long-form documents, a Siamese structure where the sub-networks are MASH RNNs is used.

* Given a source document $d_s$ and a candidate document $d_c$, they are passed to the two MASH RNN subnetworks to obtain their representations.

* While two sub-networks can be handled by an attention or similarity matrix, their utilization for long-form documents involves an enormous number of parameters. Thus, the two representations are concatenated and passed through two MLP layers in sequence, the first with a RelU activation and the second with a sigmoid activation to generate the probabilistic similarity score between the source and candidate document.

* **Predefined similarity functions such as cosine similarity do not seem to work with long-form documents**.

## Learning and Optimization

* The task of semantic text matching is modeled as a binary classification problem and as such uses a binary cross entropy loss.
