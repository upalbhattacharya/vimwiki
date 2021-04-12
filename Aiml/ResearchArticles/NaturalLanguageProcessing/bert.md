# BERT: Pre-training Deep Bidirectional Transformers for Language Understanding

## Abstract

* BERT is designed to pre-train deep bidirectional representations from unlabeled text by jointly conditioning on both left and right context in all layers.

* As a result, the pre-trained BERT model can be fine-tuned with just one additional output layer to creater state-of-the-art models for a wide range of tasks.

## Introduction

* Current techniques restrict the power of the pre-trained representations with the major limitation being that **standard language models are unidirectional**.

* Bidirectional Encoder Representations from Transformers alleviates the unidirectionality contraint by using a masked language model pre-training objective. **The masked language randomly masks some of the tokens from the input and the objective is to predict the original vocabulary ID of the masked word based only on its context**.

* The MLM objective enables the representation to fuse the lefft and right context allowing to pretrain a deep bidirectional Transformer.

* In addition to the MLM, a next sentence prediction task is also used that jointly pre-trains text-pair representations.

## BERT

* There are two steps in the framework:
	* pre-training
	* fine-tuning

* During pre-training, the model is trained on unlabeled data over different pre-training tasks.

* For fine-tuning the BERT model is first initialized with the pre-trained parameters and all of the parameters are fine-tuned using labeled data for the task.

* A distinctive feature of BERT is unified architecture across different tasks. There is minimal difference between the pre-trained architecture and the final task-specific architecture.

#### Model Architecture

* BERT's model architecture is a multi-layer bidirectional Transformer.
* Let $L$ be the number of Transformer layers, $H$, the dimension of the hidden output and $A$ the number of attention heads.

* BERT_base has $L=12, H=768, A=12$ with 110M trainable parameters while BERT_large has $L=24, H=1024, A=16$ with 340M trainable parameters.

#### Input/Output Representations

* To make BERT handle a variety of downstream tasks, **the input representations is able to unambiguously represent both a single sentence and a pair of sentences in one token sequence**.

* A sentence can be an arbitrary span of contiguous text rather than an actual linguistic sentence.

* WordPiece embeddings with a 30000 tokens is used as a vocabulary.

* The **first token is always a special classification token [CLS]**. **The final hidden state corresponding to this token output by the model is usedas teh aggregate sequence representation for classification tasks**.

* Sentence pairs are packed together into a single sequence. the sentences are differentiated in two ways:
	* A special separation token [SEP]
	* A learned embedding for every token indicating whether it belongs to sentence A or sentence B.

* For a given token, **its input representation is constructed by summing**:
	* **the corresponding token embedding**.
	* **segment embedding(sentence A or sentence B)**
	* **position embedding**

* The final hidden layer token outputs for each token can be used for a variety of tasks in the fine-tuning stage. In the pre-training stage, it is fed to a softmax to predict the masked word id.

### Pre-training BERT

* BERT pre-trains two unsupervised tasks:

#### Task 1: Masked LM

* While deep bidirectional models can be considered more powerful than left-to-right or right-to-left modls, bidirectional conditioning allows each word to indirectly see itself allowing the model to trivially predict the target word in a multi-layered context.

* In order to train the deep bidirectional representation, some percentage of the input tokens are masked at random and then predicted as the output task in pre-training.

* 15% of all WordPiece tokens in each sequence are masked at random with [MASK] with an aim for prediction of the token. However, while this is applicable in the pre-training stage, in the fine-tuning stage, such [MASK] tokens might not appear.

* To mitigate the issue of no [MASK] in fine-tuning, the masked words are not always replaced with the actual [MASK] token. When a token is chosen it is:
	* replaced with the [MASK] token 80% of the time.
	* replaced with a random word 10% of the time.
	* Not replaced 10% of the time.
 The original token is then predicted from the final hidden layer output using cross entropy loss.
 
#### Task 2: Next Sentence Prediction

* Many important downstream tasks such as Question Answering are based on understanding the relationship between two sentences which is not directly captured by language modelling.

* In order to train a modelthat understands sentence relationships BERT is pre-trained for a binarized next sentence prediction task.

* When choosing sentences A and B for each pre-training example, 50% of the the time B is the actual next sentence of A and 50% of the time it is taken from elsewhere in the corpus. The final hidden output corresponding to the [CLS] token is used for the next sentence prediction task.

#### Pre-training data

* The BooksCorpus of 800M words and English Wikipedia of 2500M words is used as the pre-training corpus with only text passages. Document level corpuses are used to make the model aware of sentence relations.

### Fine-tuning BERT

* Fine-tuning is fairly straightforward.
* The task-specific inputs and outputs are fed into the pre-trained BERT and fine-tuned end-to-end.

* The outputs from the final hidden layer for the tokens can be fed into an output layer for token level tasks and the hidden layer for the [CLS] token is fed into an output layer for classification.

* Compared to pre-training, fine-tuning is relatively inexpensive.
