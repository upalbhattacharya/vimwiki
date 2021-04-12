# Automatic Catchphrase Identification from Legal Court Case Documents
## Tags

:catchphrase:legal:

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
 
#### MyScore

* Unsupervised method for ranking sentences containing catchphrases in legal documents.
*  Combination of TF-IDF, TF and a new score named NOccur.
* In NOccur, important sentences are marked within a document. The words that appear in these sentences are weighed higher than the remaining words.

### Proposed Scoring Method

* The proposed method aims at **estimating the importance of a term specifically in the legal domain relative to a non-legal domain**.

* To represent the non-legal domain, the work uses a collection of documents from the **20 Newsgroup dataset** comprising of 18000 newsgroup posts of 20 different topics.

* For each term $t$, its importance in a domain $C$ is calculated as:

						 CF(t,C)
	Importance(t, C) = -----------
						DF(t,C) + 1

 where CF(t,C) is the frequency of $t$ in the corpus $C$ and DF(t,C) is the document frequency in $C$.
* Terms that are more important in the legal domain and less important in the non-legal domain are given a higher score. The scoring function is:
	
							 Importance(t, C_l)
	Score(t, C_l, C_nl) = ------------------------
						   Importance(t, C_nl) + 1
						   
where $C_l$ is the legal domain corpus and $C_nl$ is the non-legal domain corpus. If a term is not available in a corpus, its Importance value is given as zero.

* Having assigned scores to terms, the score for candidate phrases is calculated as:
	
	PS(c, C_l, C_nl) = log [ sum_(1 to |c|) Score(t_1, C_l, C_nl)] . KLI(c, d)
	
* The KLI term gives the specificity of the catchphrase in a legal document while the log term gives the specificity of the term in the legal domain.

* Additional importance can be given to candidate phrases that contain a legal phrase. A large dictionary of legal terms is utilized for this and the scoring is given by:
	
													  | MaxMATCH(p,c) |
	PSLegal(c, C_l, C_nl) = PS(c, C_l, C_nl)( 1 + -----------------------
															| c |

 where MaxMatch gives the maximum match the phrase $c$ has with any legal phrase $p$ in the legal dictionary.
