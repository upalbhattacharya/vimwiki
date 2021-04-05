# Charge Prediction with Legal Attention

## Abstract

* Uses an attention-based relevant article extractor to improve the performance and interpretability of charge prediction.

* Works in a bidirectional approach:
	* first, fact descriptions are used to extract relevant articles.
	* second, the selected relevant articles assist to locate key information from the fact description.

## Introduction

* Challenges faced in real-world charge prediction:
	* **Multi-Label Cases**: Real world cases are complex and diverse involing different laws and charges. Models are required to be able to predict multi-label charges.
	
	* **Interpretability**
	
## Method

### Fact Document Encoder

* The fact document encoder takes a fact description as input and outputs a fact representation sequence $d^f = {d^f_1, ... d^f_Tf}$ for a document of length $Tf$.

* An embedding layer is used to convert the textual input fact description into an embedding sequence $x^f = {x^f_1, ... ,x^f_Tf}$ where each word is given by a corresponding word embedding vector.

* A CNN encoding scheme is used to encode the text from the embeddings with padding to ensure the output sequence is also of the same length as the number of word embeddings inputted. For each word, the concatenation of the corresponding CNN filter outputs gives the document embedding vector for that word

### Relevant Article Extractor

* A max pool is applied to the document representation $d_f$ to obtain the fact representation $e^f$.
* The article is then scored by an MLP layer with a sigmoid activation.

### Article Document Encoder

* Uses the same architecture as the fact document encoder.

### Attention-based Charge Prediction

* 
