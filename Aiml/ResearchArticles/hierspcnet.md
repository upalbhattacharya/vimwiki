# Hier-SPCNet: A Legal Statute Hierarchy-based Heterogeneous Network for Computing Legal Case Document Similarity

## Abstract

* All prior network-based similarity methods consider a precedent citation network among case documents only (PCNet). This approach misses an important source of legal knowledge: the hierarchy of legal statutes that are applicable in a given legal jurisdiction.

* Augments PCNet with the hierarchy of legal statutes to form a heterogeneous network having citation links between case documents and statutes as well as citation and hierarchy links among statutes.

## Introduction

* In the Common Law System, there are two primary sources of law:
	* Statutes or Written Laws(e.g. sections in the Penal Code outlining punishments for different crimes)
	* Precedents or prior cases decided by important courts.
* Issues with legal documents: long, complicated and unstructured.
 
* No well-defined notion of similarity between legal documents.

* General supervised document similarity measures cannot be used for legal documents as there is no standardized dataset to use for training. Thus, most methodologies for finding similarity between legal documents are unsupervised.

* Broad classification of methods for computing legal document similarity:
	* **network-based methods**: rely on citation to prior case documents
	* **text-based methods**: rely on textual content of the documents
	* **hybrid**

* This work focuses on network-based approaches.
* PCNet only captures citation from one case document to prior case documents.

* To estimate similarity between legal documents, uses graph embedding algorithm **Metapath2vec** on Hier-SPCNet.

* The notion is that if two documents cite the same statute or different statutes/precedents that are structurally similar in the network then they may be discussing similar legal issues.

## Existing Network-Based Methods for Legal Document Similarity

* Existing network-based similarity methods construct a PCNet in which the vertices are case documents and there is directed edge $d1->d2$ if the document $d1$ cites document $d2$.

* Existing similarity measures applied on PCNet are:
	* **Bibliographic Coupling**: **Jaccard similarity index** between the sets of precedent citations(out-citations) from the two documents who similarity is to be inferred.
	* **Co-citation**: Similar to Bibliographic Coupling but uses in-citation.
	* **Dispersion**: Measures to what extent the out-citation neighbours of two documents are themselves similar.

## Proposed Augmentation of PCNet with Legal Statute Hierarchy

### Constructing Hier-SPCNet

#### Modeling the hierarchy of statues

* An act has a hierarchy of structure as:
	$Act -> Parts -> Chapter -> Topics -> Section/Article$

* Smaller acts may not strictly adhere to this hierarchy by skipping certain levels of it.

* For Hier-SPCNet, hierarchy is extracted from a text of statutes and represented as a structure of nodes(corresponding to Acts, Parts, Chapters, etc.)

#### Extraction of citations from text

* Citations are extracted based on regular expression searches.

#### Hier-SPCNet

* The network consists of **six types of nodes**:
	* case documents
	* acts
	* parts
	* chapters
	* topics
	* sections/articles

* There are **two types of edges/links**:
	* Citation edges:
		* $document -> document$ (PCNet)
		* $document -> statute$
		* $statute -> statute$
	* Hierarchy edges:
		* Can be of various types:
			$Act -> Parts -> Chapter -> Topics -> Section/Article$
		 Can be the whole link or any subset of the link chain with missing nodes in between as well.
		 
### Document similarity using Hier-SPCNet

* The similarity measures described before can be applied to Hier-SPCNet with the added inclusion of statute information.

* Graph embedding techniques are used to map the nodes of the graph to a vector space such that nodes having similar neighbourhoods in the network have similar representations. The **cosine similarity** between the embeddings gives the similarity between the documents.
	* **Node2Vec**: Node2Vec generates embeddings via random walks following BFS or DFS. Node2Vec assumes the network to be homogeneous which is true in the case of PCNet but not for Hier-SPCNet. It is used under the assumption of homogenity for Hier-SPCNet.
	* **Metapath2Vec**:
		* It is meant for heterogeneous networks.
		* Similar to Node2Vec but works on user-defined metapaths where a metapath is a path between two nodes where the edges can have different meaning.
		* Hier-SPCNet uses 14 different metapaths:
			* doc-sec-doc: Two documents citing the same section/article
			* doc-sec-topic-sec-doc: Two documents citing different sections/articles but of the same topic.

## Experiments and Results

### Experimental Setup

* **Dataset used**: 1806 case documents and 128 acts that are cited by at least one of the documents is used.
* **Developing gold standard for document similarity**:
	* two legal experts scored 100 document-pairs from 0.0(dissimilar) to 1.0(similar).
	* Average of scores from both legal experts used.
* **Evaluation metric**: Pearson correlation used for similarity between expert rated scores and computed scores.

### Results: PCnet vs. Hier-SPCNet

* Across various network-based methods, Hier-SPCNet showed statistical improvement over PCnet.
* The value of co-citation remains the same for both networks as it depends on the common in-citations which stays the same for both methods.
* Node2Vec based similarity computation also showed significant improvement despite considering the graph to be homogeneous. This is because the hierarchial structure of statutes over PCNet helps since the leaf nodes i.e. section nodes are structurally similar.
* Metpath2vec showed the best performance.

## Comparing Network-Based and Text-Based Similarity

* text-based method using Doc2Vec achieves a correlation of 0.734 with the mean expert similarity as opposed to 0.674 with the network-based method. The difference is not statistically significant.
* Combining text-based and graph-based may help as they probably capture complementary signals of legal document similarity.
