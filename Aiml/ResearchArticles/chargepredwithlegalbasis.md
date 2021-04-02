# Learning to Predict Charges for Criminal Cases with Legal Basis

## Abstract

* Proposes an attention-based neural network method to jointly model charge prediction and relevant article extraction in a unified framework.

## Introduction

* Predict appropriate charges based on fact descriptions can be non-trivial:
	* **The differences between two charges can be subtle** requiring careful understanding of the facts.
	* **Multiple crimes may be involved in a single case thus requiring multi-label classification of charges**

* Most problems treat charge prediction and relevant article extraction independently ignoring that one task could benefit from the other.

* Uses a two-stack attention mechanism:
	* **a sentence-level and document-level BiGRU with a stack of fact-side attention components to model the correlation among words and sentences for a document**.
	
	* **given the fact description, accordingly learn a stack of article-side attention components to attentively select the most supportive law articles that support a charge prediction**.

## Related Work

* It is hard and expensive to match facts related to different defendants with their corresponding charges.

## Approach

### Document Encoder

* Considering a sentence as a sequence of words and a document as a sequence of sentences, the document embedding problem is considered as two sequence embedding problems.

* First, each sentence is embedded using a sentence-level encoder which is then further aggregated with a document-level sequence encoder to produce a document embedding $d$.

* The document and sentence-level encoder can have different architectures but are modeled with the same architecture as a BiGRU with attention.

* This leads to two attention-based context vectors: the word-level $u_fw$
 and sentence-level $u_fs$.
 
 ### Using Law Articles
 
* Uses a two-step approach by first selecting the top-k relevant articles and then using article-side attention to select the most supportive ones for charge prediction.

	* **Top k Article Extractor**:
		* Uses a multiple binary classifier for each article to extract relevant articles.
		
		* Uses a word-based SVM for the binary classifier.
		
		* Used BOW TF-IDF features, chi-square for feature selection and linear kernel for binary classification.

	* **Article Encoder**:
		* For each article in the top-k extracted articles, a document embedding is created using the same architecture as the document/fact encoder.
		* It differs from the fact encoder in that the global word and sentence level context vectors are dynamically generated for each case corresponding to its corresponding fact embedding $d_f$ as:
			
			$u_aw = W_w d_f + b_w$
			$u_as = W_s d_f + b_s$
			
		* The fact embbedding $d_f$ guides the model to attend to informative words or sentences with respect to the facts of each case.
		
	* **Attentive Article Aggregator**:
		* a bi-RNN attention based architecture is used to generate a the aggregated article embedding $d_a$.
		
	* **Output**:
		* The fact embedding and aggregated article embedding are concatenated and pass through two fully connected layers followed by a softmax to produce the predicted classification.
		* **Supervised Article Attention** can be used to enhance the standard cross entropy loss function by including a cross-entropy term for correct prediction of law articles with respect to the target article distribution taken as uniformly distributed for the top-k extracted articles.
