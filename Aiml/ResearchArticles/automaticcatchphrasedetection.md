# Automatic Catchphrase Identification from Legal Court Case Documents

## Abstract

* Proposes an unsupervised approach for extraction and ranking of catchphrases from legal documents by focusing on noun phrases.

## Introduction

* The common law system places great emphasis on prior cases for interpretation required in ongoing cases.

* Catchphrase can help lawyers in quickly understanding the underlying legal concepts pertaining to a case.

### Challenges

* Catchphrases occur in low numbers in court case documents and can occur in only a few documents in a large collection.

* They are of variable lengths.

* Presence of stopwords in catchphrases prevent removing stopwords as a preprocessiing step prior to retrieval.

### Present Work

* Presents an automated methodology for extraction of catchwords from legal documents.

* Proposes a novel scoring function for ranking the extracted catchphrases.

## Methodologies for Catchphrase Extraction

* The task of automatic catchphrase extraction can be divided into two sub-tasks:
	* **Selection of candidate catchphrases**: Searching a given document to identify parts of the document text that can be potential catchphrases.
	* **Scoring and ranking of candidate catchphrases**: Assigning scores and ranking the candidate catchphrases.

### Selection of candidate catchphrases

* Manual inspection yields that most catchphrases in legal documents are noun phrases.

* Verified through empirical verification by viewing the catchphrase extraction task as a pattern matching problem. Experimenting with word n-grams and noun phrases from legal texts showed that noun phrases had better matching with catchphrases that are actually identified by legal experts.

* Noun phrases therefore are used as candidate catchphrases and extracted using a standard noun phrase chunker. 
  
* Extraction of noun phrases requires a grammar that defines the structure of the noun phrase. The standard noun phrase in English can be quite complicated and a simplified grammar involving only adjectivves, nouns and prepositions is used.

* While patterns extracted from a document can be in singular or plural senses, legal catchphrases are usually in singular. To address this issue, a Parts-of-Speech tagger is used to identify nouns in the extracted pattern and lemmatization is used on the nouns to remove inflectual endings. The same is not used for other words as inflectual endings in other parts of speech occur in legal catchphrases.

### Scoring of the selected candidate phrases

#### BM25

* Scores a candidate catchphrase $c$ in a document $d$.

	Score (c,d) = IDF(c) .          TF(c,d).(k_1+1)
							-----------------------------------
							TF(c,d) + k_1 . (1 - b + b.  |d| )
														------
														avg(dl)
 where TF(c,d) is the frequency of the candidate frequency of $c$ in $d$, $k_1$ and $b$ are free parameters and IDF(c) is the inverse document frequuency of the phrase $c$
	
	IDF(c) =     n - DF(c) + 0.5
			 log ----------------
				   DF(c) + 0.5
 
 where DF(c) is the Document Frequency or the number of documents where $c$ appears.
 
#### KLIP

* It is a KL-divergence based score that is the sum of two different scores:
	* **KL Informativeness (KLI)**: Measures how well a candidate phrase $c$ represents a document $d$.
	
	                               TF(c, d)
							       ---------
			     TF(c,d)		     | d |
	KLI(c, d) = ---------- . log -------------
				   | d |             CF(c)
									-------
									   n
 where CF(c) is the total number of occurences of $c$ in the whole corpus.
	
	* **KL Phraseness (KLP)**: Used for multiword phrases. It compensates for the low frequency of multi-word phrases by assigning higher weights to longer phrases.
	
	                                    TF(c, d)
							           ---------
			     TF(c,d)		         | d |
	KLP(c, d) = ---------- . log ---------------------
				   | d |        ___   Freq(t_i, d)
								| |	 --------------
							   1->|c|	  |d|
							   
 where $t_i$ is the $ith$ unigram in the catchphrase $c$ and $Freq(t_i, d)$ is the frequency of $t_i$ in $d$.
 
 
