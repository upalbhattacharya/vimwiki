# Charge Prediction with Legal Attention

## Tags

:legalchargeprediction:legal:attention:interpretability:

## Abstract

* Uses an attention-based relevant article extractor to improve the performance and interpretability of charge prediction.

* Works in a bidirectional approach:
	* first, fact descriptions are used to extract relevant articles.
	* second, the selected relevant articles assist to locate key information from the fact description.

## Introduction

* Challenges faced in real-world charge prediction:
	* **Multi-Label Cases**: Real world cases are complex and diverse involing different laws and charges. Models are required to be able to predict multi-label charges.
	
	* **Interpretability**
	
* The defined model simulated the charge prediction process in the civil law system by its bidirectional approach.

## Method

* The method uses fact description to extract relevant articles and in turn, uses the relevant articles to focus on the most important parts of the fact description.

* Given a fact description as input, its fact representation sequence $d^f$ is given by the model.

* An article document encoder generates an article representation sequence $d^a$ for each relevant article.

* The article representation sequences are fed into the attention layer to calculate the attention-based fact representation $e^f_final$ which is used to predict the appropriate charges for the input case.
 
### Fact Document Encoder

* The fact document encoder takes a fact description as input and outputs a fact representation sequence $d^f = {d^f_1, ... d^f_Tf}$ for a document of length $Tf$.

* An embedding layer is used to convert the textual input fact description into an embedding sequence $x^f = {x^f_1, ... ,x^f_Tf}$ where each word is given by a corresponding word embedding vector.

* A CNN encoding scheme is used to encode the text from the embeddings with padding to ensure the output sequence is also of the same length as the number of word embeddings inputted. For each word, the concatenation of the corresponding CNN filter outputs gives the document embedding vector for that word

### Relevant Article Extractor

* A per-word/word representation max pool is applied to the document representation $d^f$ to obtain the fact representation $e^f$ as:
	
	$e^f_i = max(d^f_1i, d^f_2i, ...d^f_Tfi) i in [1, m]$ 
	
	where $m$ is the feature size
	
* The article is then scored by an MLP layer with a sigmoid activation.

* True relevant article labels are provided during the training step while in prediction, only articles with a score higher than a given threshold are considered truly relevant.

### Article Document Encoder

* Uses the same architecture as the fact document encoder but with different parameters as fact descriptions and relevant articles have different emphases.

### Attention-based Charge Prediction

* Given a fact description $d^f$ and an article representation $d^a$, the article representations are used to focus on different parts of the fact description through attention.

* **Legal Attention**:
	* Attention is implemented as a query retrieval mechanism using document representation word embeddings as keys and the corresponding article representation word embeddings as queries. The keys and queries are given as:
		$k_i = tanh(W^k d^f_i) i in [1, Tf]$
		$q_i = tanh(W^q d^a_i) i in [1, Ta]$
		
	* The $Ta x Tf$ legal attention matrix is then given by:
		
		$A = softmax(alpha_ij), alpha_ij = dot(q_i, k_j)$
		
	* Applying legal attention with the legal attention weights $alpha_ij$ gives the fact description with legal attention:
		
	* A max pool is applied over the fact description sequence with legal attention to get the representation $e^f_legalattn$.

* **Attention from Different Articles**:
	* Due to the multi-label property of the problem, the relevant article extractor yields more than one relevant article.
	 
	* The final fact representation is given by taking the averages of the fact representations computed using legal attention of a fact description with each article.
	
	* A residual connection is used to reduce the impact of irrelevant articles.

* **Charge prediction**:
	* The final representation $e^f_final$ is pased through an MLP layer with sigmoid activation for charge prediction.

### Training

* The loss of the model consists of two cross entropy losses:
	* one for the charge prediction
	* one for the the relevant article extraction based on the scores given by the relevant article extractor.

* The final loss is a weight sum of the charge cross entropy loss and the relavant article extractor cross entropy loss as:

	$L = L_charge + alpha L_article$
	
