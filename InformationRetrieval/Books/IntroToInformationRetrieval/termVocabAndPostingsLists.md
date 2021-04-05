# The term vocabulary and postings lists

* Defining a document unit depends on the nature of the use-case or problem to be handled.

* Generally, for very long documents, the issue of **indexing granularity** arises. **Choosing an appropriate granularity**(e.g. choosing an entire book as a unit vs. choosing chapters as units vs. choosing sentences as units) is a case of **precision/recall tradeoff**.

## Tokenization

* **Definition(Tokenization)**: Given a character sequence and a defined document unit, tokenization is the task of chopping it up into pieces called **tokens**, and possibly also throwing away certain characters such as punctuation.

* A **token** is an instance of a sequence of characters in some particular document that are grouped together as a useful semantic unit for processing.

* A **type** is the class of all tokens containing the same character sequence.
* A **term** is a normalized type that is included in an IR system's dictionary.

* Choosing a proper tokenization strategy is a major task and can vary from language to language. Chopping on whitespaces can be an initial idea but may also have several problems later.

* For either boolean or free text querires, the exact same tokenization should be applied to both the documents and query words. This guarantees that a sequence of characters in a text will always match the same sequence in a query.

* The issues of tokenization are **language-specific**. Thus, the language of the document is required to be known.

* Language identification based on classifiers that use short character subsequences as features is highly effective as most languages have distinctive signature patterns.

* Languages and particular domains have unusual specific tokens that should be recognized as terms and as such add semantically meaningful information to the document so as to not be discardable without significant cost of restricting what can be searched.

* Tokenization of **hyphenated words** can be an issue in the English language with hyphenation having a **multi-faceted interpretation**. For example, in a copyediting device to show word grouping as 'the hold-him-back-and-drag-him-away maneuver', splitting along hyphens can be effective whereas in the usage of hyphenation in 'co-education' treating it as a single word 'coeducation' might be effective. 

* Handling **apostrophes** in the English language can be tricky with separation along apostrophes leading to intuitively poor tokens in case of words like "aren't".

* Splitting along whitespaces can end up splitting what should be considered as a single token such as names of cities like 'Los Angeles' or borrowed foreign phrases 'au fait' or even words that may sometimes be written as two words (white space vs. whitespace). Such splitting can give poor results.

* A possible manner of handling hyphenation issues is by allowing the system to query with all three variants:
	* hyphenated word
	* single word
	* two words

* **Compound words** like in the German language can utilize **compound splitters** which check if a word can be subdivided into multiple words that appear in a dictionary.

## Stop words

* **Definiton(Stop words)**: Stop words are a collection of extremely common words which would appear to be of little value in helping select documents matching a user need and thus can be excluded from the vocabulary entirely.

* The general strategy for determining a stop list is by sorting the terms by collection frequenct and then taking the most frequent terms as the stop list.

* Using a stop list **significantly reduces the number of postings that a system has to store**.

* While not indexing stop words does little harm most of the time as keywords like 'the' and 'to' are not very useful in a query, in some situations they can be relevant such as "The President of the United States" or "flights to London" where removal of stop words can potentially change the meaning of the phrase.

## Normalization

* **Definition(Token normalization)**: Token normalization is the process of canonicalizing tokens so that matches occur despite superficial differences in the character sequences of the tokens.

* The most common way to normalize is to create **equivalence classes**. This is most commonly done using mapping rules that remove characters like hyphens. Here, the equivalnce classing to be done is implicit.

* An alternative normalization strategy is to maintain relations between unnormalized tokens. The usual way to do this is to index unnormalized tokens and to maintain a query expansion list of multiple vocabulary entries to consider for a certain query term. A query term is then effectively a disjunction of several postings lists. The alternative is to perform the expansion during index construction. Use of either of these methods is considerably less efficient that equivalence classing as there are more postings to store and merge.

* However, the alternative strategies are more flexible than equivalnce classes because the expansion lists overlap while not being identical.

* While utilizing a balance of equivalence classes and expansion lists is a good idea, the relative proportions to use is not well defined.

* While one can worry about the various details of equivalence classing, a consistent processing of the query and the documents ensures that there is not much aggregate effect on performance.

### Commonly implemented forms of normalization

* **Accents and diacritics**:
	* Diacritics in the English language is fairly marginalized and can be removed without much consequence.
	
	* In contrast, in languages where diacritics may result in a change in the meanings of words, removal of diacritics may pose issues. However, given that most queries given by users may err on using diacritics, equating all words to forms without diacritics is considered the best practice.

* **Capitalization/case-folding**:
	* A common case-folding strategy is to reduce all letters to lower case.
	
	* Case-folding by conversion to lower case can help with inaccurate queries posted by users.
	
	* However, conversion to lower case can create issues by equation words that semantically mean different things, particularly proper nouns. In conjunction with query expansion, this can create very undesirable conditions such as equating "C.A.T" with "cat".
	
	* An alternative method would be to convert only some words to lower case. A simple heuristic would be to convert to lower case words at the beginning of a sentence all words occuring in a title that is entirely upper case or in which most of all words are capitalized. This allows for mid-sentence capitalized words to be retained as is allowing for distinctions to be made between different use cases of words/phrases as in proper nouns. This is known as **truecasing** and can be more effectively done through machine learning.
	
	* More elaborate case-folding scenarios may be ineffective if the queries made do not respect capitalization rules of the language.

* **Other issues in English**:
	* These can include spelling variations and other contemporary literary variations.
	* Different formats for writing calender dates and time.

* **Other languages**:
	* Other languages can have other intricacies that require careful handling during normalization. 
	  * The male, female and neutral variations of articles in languages such as French and German can be normalized to their English standard forms. 
		
## Stemming and lemmatization

* Documents contain different forms of a word and families of derivationally related words with similar meanings. In many situations, it may be useful to return documents containing such related words to a word in the search query.

* **The goal of both stemming and lemmatization is to reduce inflectional forms and sometimes derivationally related forms of a word to a common base form**. However, they differ in their behaviour.

* **Stemming** is a crude heuristic process that **chops off the ends of words** in the hope of achieving this goal correctly most of the time and often includes removal of derivational affixes. 

* **Lemmatization** refers to doing things properly with the **use of a vocabulary and morphological analysis of words normally aiming to remove inflectional endings only and to return the base or dictionary form of a word** which is known as a **lemma*.

* As an example, stemming of the word 'saw' would yield 's' while lemmatization may yield 'see' or 'saw' depending on the usage context as a noun or a verb.

* Stemming most commonly collapses derivationally related words whereas lemmatization commonly only collapses the different inflectional forms of a lemma.

* The most common algorithm for stemming English is **Porter's algorithm**:
	* It consist of 5 phases of word reductions applied sequentially.
	* Within each phase, there are various conventions to select rules.
	* Many of the later rules use a concept of the **measure** of a word which loosely checks the number of syllables to see whether a word is long enough that it is reasonable to regard the matching portion of a rule as a suffix than as part of the stem of a word. An example is converting 'replacement' to 'replac' but retaining 'cement' as is.
	
* **Stemming increases recall while harming precision**.

## Positional postings and phrase queries

* Many complex or techical concepts as well as names are multiword compounds or phrases. Queries themselves can be phrases that have meaning in the whole that is separate from the meanings of the individual words constituting the query phrase such as "Stanford University" as opposed to "Stanford" and "University", the latter could be in reference to individuals of the name "Stanford" and any university.

* To be able to support phrase/multiword queries, it is no longer sufficient for postings list to be simply lists of documents that contain individual terms.

### Biword indexes

* Biword indexes handles phrases by **considering every pair of consecutive terms in a document** as a phrase.

* Each biword is treated as a vocabulary term. While two-word phrases can be processed immediately, longer phrases need to be broken down in order to be processed.

* Among possible queries, **nouns and noun phrases** have a special status in describing the concepts people interested in searching for. 

* However, related nouns can often be divided from each other by various function words. To properly incorporate these into the biword indexing model, the following procedure is used:
	* **tokenize** the text and perform **part-of-speech-tagging**.
	* **group terms** into nouns and proper nouns (N) and function words including articles and prepositions (X).
	* Deem any string of terms of the form NX*N to be an **extended biword**.
	* Any query using an extended biword is to be parsed in a similar manner into Ns and Xs and then segmented into extended biwords.
	* More precise POS patterns can lead to better results.

* The concept of biword index can be extended to longer sequences of words and in the case of variable length word sequences, is generally referred to as **phrase index**.

* Searches for single words are not handled naturally in a biword index requiring an additional single word term index.

* While longer phrase indexing can reduce false positives, storing longer sequences has the potential to greatly expand the vocabulary size. Even storage of an exhaustive biword dictionary greatly expands the vocabulary size and can be problematic.

## Positional indexes

* A biword index is not the standard solution.

* A **positional index** is commonly employed in which, for each term of the vocabulary, a posting(i.e. for a particular document) is of the format:
	 
		docID: <position1, position2, ...>
	
	where each position is a token index in that document. Each posting usually also records the term frequency.
	
* Processing a phrase query still requires accessing the inverted index entries for each distinct term.

* In the merge operation, the same general technique is used as before for a query with the exception being to check that the positions of appearence of the terms in the document are compatible with that in the query as opposed to simply checking for their existence in the document. The postional aspect of the check requires working out offsets between the words.

* within-k-word proximity searches follow the same general method.

* **Positonal indexes significantly expand the required postings storage**.
* A rough estimate puts a postional index to be 2 to 4 times as large as a non-positional index

## Combination schemes

* The strategies of biword indexes and postional indexes can be fruitfully combined.

* A combination strategy uses a phrase index for certain queries and positional indexes for other queries.

* Commonly used phrases can easily be retained as phrase queries instead of constant recomputation through positional indexes that can be significantly more expensive.

* Good queries to include in the phrase index are ones known to be common based on recent query behaviour.

* The most expensive queries are ones where the individual words are common but the desired phrase is comparatively rare.
