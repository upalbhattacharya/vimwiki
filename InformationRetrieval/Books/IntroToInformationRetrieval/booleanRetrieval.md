# Boolean Retrieval

**Definition(Information Retrieval)**: Information Retrieval is finding material of an unstructured nature that satisfies an information need from within large collections.

* Information retrieval can also include browsing and filtering documents and further processing on a set of retrieved documents. An example of this would be clustering.

* Information retrieval can be categorized according to their scale with each scale having certain different problems and approaches:
	* **web search or large scale**: Issues include:
		* Needing to gather documents for indexing
		* efficient large systems
	
	* **personal IR**: Issues include:
		* Handling a broad range of document types
		* making the search system maintainence free and sufficiently lightweight
		
	* **enterprise, institutional and domain-specific**
	
* **For small documents, linear scan** through the document or grepping might be sufficient(documents around 1 million words) for simple tasks like finding matching terms.

* However, for larger collections of data or for more flexible tasks and ranked retrieval, grepping is impractical.

* **Repeated linear scanning through a document for every query can be avoided by indexing the documents in advance**.

* **Definition(Term-document incidence matrix)**: A binary-term matrix whose entries indicate the existence of a term in a document.

* **Definition(Boolean Retrieval Model)**: The boolean retrieval model is a model for IR in which any query can be posed in the form of a boolean expression of terms which can be combined by the operators AND, OR and NOT.

* **The most standard IR task is ad hoc retrieval**. In an ad hoc retrieval, a system aims to provide documents from within a collection/corpus that are relevant to an arbitrary user **information need**.

* An **information need** is the topic about which the user desires to know more and is different from a **query** which is what the user conveys to the computer in an attempt to communicate the information need.

* For a large collection of documents, it is not possible to build a term-document matrix that will feasibly be accomodated in memory.

* The term-document matrix is extremely sparse. As a results, it is better to have a representation that only records instances/terms that do occur.

* **Definition(Inverted index)**: An inverted index is a more efficient term incidence representation for a large corpus. It utilizes a **dictionary of terms that appear in the corpus** and, **against each term, maintains a postings list containing the documents that contain that term**. All the postings lists together is refered to as a postings or an inverted index. Each item(document) in a term's corresponding postings list is called a posting.

* Creating an indexing involves:
	* Assigning unique documents IDs to each document in a corpus
	* Tokenizing and splitting terms in all documents and treating each term as a term-document ID pair
	* sorting the term-document ID pairs alphabetically
	* merging multiple occurences of the same term **from the same document**
	* grouping same terms across different documents
	* splitting the pairs into a dictionary of terms and postings
	* sorting each posting by document ID

* **Inverted index** is the most efficient structure for supporting ad hoc text search.

* Usually the dictionary is kept in memory while the larger postings lists are normally kept on disk.

* Processing boolean queries with a given inverted index involves:
	* retrieving relevant postings from the given query
	* intersecting the postings list based on the logical operations needed for the query

* Efficient intersection of postings lists is done by merge sort.

* **Query optimization** is the process of organizing the work of answering a query for least system work. A major element of this for Boolean queries involves the order of accessing postings.
