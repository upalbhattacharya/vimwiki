# BERT-PLI: Modeling Paragraph-Level Interactions for Legal Case Retrieval

## Tags

:legalcaseretrieval:bert:languagemodel:

## Abstract

* Legal case retrieval is harder than regular text retrieval as queries are much longer and complex.

* Relevance between a query case and a supporting case is beyond general topical relevance and is therefore difficult to construct large-scale retrieval datasets.

* BERT-PLI utilizes BERT to capture semantic relationships at a paragraph level and infers the relevance between two cases by aggregating paragraph-level interactions.

## Introduction

* Existing methods for information retrieval tasks face a few serious challenges in the context of legal case retrieval:
	* **Both query and candidate cases involve very long texts**: It is challenging for representation learning methods to represent the long document well in a limited semantic space.
	
	* **The definition of relevant cases goes beyond topical relevance**: It can include cases that support decisions in a current case.
	
	* **Lack of a large datasets**

* BERT-PLI breaks a document into paragraphs and infer the relationship between cases from a fine-grained perspective which allows usage of the entire document instead of a summarized or truncated document. Semantic interactions between paragraphs are modeled.

* BERT-PLI is adapted to the legal scenario by fine-tuning on a sentence pair classification task with a small-scale legal case entailment dataset.

## Method

### Task Description

* Given a query cas $q$ and a set of candidate cases $D = {d_1, d_2, ...,d_n}$, the objective is to identify supporting cases $D* = {d_i* | d_i* in D union noticed(d_i*, q)}$ where $noticed(d_i*,q)$ denotes that the candidate case $d_i*$ is relevant to the query case $q$.

* Both query and candidate cases are legal documents containing long texts which consist of the facts of the case.

### Architecture Overview

* Deals with the legal case retrieval task within a multi-stage pipeline inspired by the cascade framework.

#### Stage 1: BM25 selection

* The top K candidates are selected from the initial candidate corpus with respect to the query case $q$ according to the BM25 scores.

* This stage hurts both recall and precision but the main focus is on optimizing recall at this stage as irrelevant documents can further be discarded in the downstream task.

#### Stage 2: BERT Fine-Tuning

* The pre-trained BERT model is fune-tuned with a small-scale legal case entailment dataset provided by COLIEE 2019 Task 2. This task involves identifying the paragraphs that entail the decision paragraph of a query case from a given relevant case.

* Fine-tuning on this task enables BERT to infer supportive relationships between paragraphs.

* All parameters of BERT and fine-tuned on a sentence pair classification task in an end-to-end fashion.

* The input is composed of the decision paragraph of a query case and a candidate paragraph in the relevant case which are separated as done in the standard BERT model. The hidden state vector correposponding to the [CLS] token for the sentence pair is used for the classification task with a binary cross-entropy loss.

#### Stage 3: BERT-PLI

* Each document is broken into paragraphs and the interactions between paragraphs are modeled.

* A query case $q$ is represented as a set of paragraphs ${pq_1, pq_2, ... , pq_N}$. Similarly, each candidate document/case $d$ is also represented as a set of paragraphs ${pd_1, pd_2, ... pd_M}$.

* All possible $NM$ paragraph combinations are used as inputs to the fine-tuned BERT model with the corresponding final hidden state vector for the [CLS] token of the input used to get an interaction score. This leads to an interaction map between all sets of paragraphs in the query and candidate document case.

* For each query paragraph, the strongest matching score with respect to the document paragraphs is captured using a max pool strategy.

* An LSTM/GRU/RNN is used to further encode the max-pooled representation sequence.

* The LSTM hidden state units are then passed through a self-attention model to get the attention weights. The final representation is obtained as a weighted sum of the hidden states of the LSTM, weighted by the attention weights.

* The weighted sum representation is passed through a fully-connected layer followed by a softmax for the final prediction.

* The model is optimized with respect to multi-class cross-entropy loss.
