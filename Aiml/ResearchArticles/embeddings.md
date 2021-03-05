# Word2Vec

* A distributed and distributional semantic word embedding model.

* Aims to present words in vector form such that similar words have higher similarity(cosine) representing semantically similar meanings of words.

* Makes use of a simple two layer neural network where there is no non-linear activation in the first layer to gain the embeddings.

* Consists of using a context window for each word that looks on either side of the center/target word to be embedded. For a context value of $r$, the methods look at $r$ words on either side of the center word in a sliding window fashion.

* The initial step includes getting a large corpus of text such as Wikipedia. Each unique word is then encoded by the **one-hot vector method**. Thus, for a corpus size consisting of $T$ words, each word representation is a $T x 1$ vector with zeros in all locations except at the index position of the word in the corpus where it is a one.

* The output of the model is to predict a probability distribution that either predicts the center word(CBOW) given the context words or predicts the context words(Skip-gram) given the center word. Thus, the output is obtained from a softmax non-linearity applied to the second layer output.

* The output of the model is **not the matter of interest**, instead, **it is the weights in the first layer after training that give the word embeddings**. The size of the word embeddings $V$ is a hyperparameter that can be tweaked to find an optimal solution. The original article used a size of 300.

* Depending on the objective, can be divided into two models:
	* **Continuous Bag of Words**:
		* Takes the word representations of the words in the window and aims to predict the center word.
		* The output is a $T x 1$ softmax output that gives the prediction of the center word.
	
	* **Skip-Gram**:
		* Takes the word representation of the center word and aims to predict the context words in the sliding window.
		* The output is $2r$ $T x 1$ softmax outputs that give the prediction of each of the context words in the sliding window.

## Continuous Bag of Words

* Takes as input the $2r$ $T x 1$ word representations in the context window and tries to predict the center word.

* The training objective is given as:
					  
				  ___T  ___j=r
	J(theta) =    | |   | |    p(w_t | w_(t+j); theta)
				  | |   | |
					t=1    j=-r
			 			   j!=0 
							  
					  
				  1  ___T  ___j=r
	J(theta) = - --- \     \      log p(w_t | w_(t+j); theta)
				  T	 /__   /__
				       t=1    j=-r
							  j!=0 
							  
* The $2r$ $ T x 1$ context word representations are either added/averaged before the first layer thus losing their positional value and thus the name.

* The first layer consists of a $T x V$ weight vector matrix $W$ and no non-linear activation that acts on the averaged/added context words' representation to give a $V x 1$ vector. 
  
* The $V x 1$ **projection obtained** then passes through the output layer where it is acted upon by a $V x T$ weight matrix $W'$ followed by a softmax non-linear activation to give a $T x 1$ probability distribution vector that acts as the prediction of the context word. 
  
* After training, the $T$ rows of the weight matrix of the first layer act as the $T$ V-dimensional word embeddings.

## Skip-Gram

* Takes as input the $T x 1$ word representation of the center word and tries to predict the $2r$ context words in its sliding window.

* The training objective is given as:
					  
				  ___T  ___j=r
	J(theta) =    | |   | |    p(w_(t+j)| w_t; theta)
				  | |   | |
					t=1    j=-r
			 			   j!=0 
							  
					  
				  1  ___T  ___j=r
	J(theta) = - --- \     \      log p(w_(t+j) | w_t; theta)
				  T	 /__   /__
				       t=1    j=-r
							  j!=0 
							  
* The first layer is again a $T x V$ weight vector matrix $W$ and no non-linear activation that acts on the $T x 1$ word representation to produce a $V x 1$ projection. 
  
* Intuitively, since the $ith$ center word is represented by a one-hot vector with a 1 in the $ith$ coordinate and zeros, elsewhere, the output of the first layer is just the $ith$ row of the weight matrix of the first layer.

* The $V x 1$ projection is acted upon by the $V x T$ matrix $W'$ followed by softmax activation to give the output context words' embeddings. The **same** weight matrix $W'$ is used to get the $2r$ $T x 1$ outputs.

## CBOW vs Skip-Gram

* While CBOW seems more intuitive and is faster to train, Skip-gram gives better results usually, specially for less frequent words. 
  
* In both the models, the rows of the first layer matrix gives the word embeddings of the center/target word. The second matrix gives the embeddings of the context words. Thus, each word has two embeddings. The embedding of the center words is used as the main embedding usually.


# Doc2Vec

* This is an extension of the word2vec model that can be used for arbitrary length texts including phrases, sentences, paragraphs and even entire documents. 
  
* It generates a Paragraph Vector. Each paragraph(here paragraph refers to any sequence of words of any length) is mapped to a unique vector. 
  
* The same model for word2vec **CBOW** model can be used with the addition of a matrix $D$ in the first layer that represents the paragraph embeddings in a similar manner as the word embedding matrix.

* The paragraph vector can be thought of as another word. The paragraph vector is shared across all contexts generated from the same paragraph but not across paragraphs. The word vector matrix $W$ is shared across all paragraphs. The training objective stays the same as before with the additional parameter $D$.
 
* This paragraph embedding obtained after the first layer can either be **added or concatenated** to the word projection vector obtained as the output of the first layer in the word2vec model.

* The model is trained with gradient descent as usual.

* For **prediction** of a new paragraph vector, the weight matrices $W$ and $W'$ are kept fixed and the model is further trained for a few iterations to get the new embedding paragraph vector corresponding to the new paragraph.

* In the word2vec model, for a corpus of $T$ words that are to be embedded in a $p$ dimensional space, there are $T x p$ trainable parameters. In the doc2vec model, in additon to this, there are $S x q$ trainable parameters corresponding to the $S$ paragraphs that are to be embedded in a $q$ dimensional space.
