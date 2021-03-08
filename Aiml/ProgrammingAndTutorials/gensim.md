# Gensim

* It is a NLP package that does topic modelling.

* Given a sequence of words as a sentence, paragraph or a document, a dictionary of words can be created by:

	```python
	import gensim
	from gensim import corpora
	
	documents = [list of strings]
	
	texts = [[text for text in line.split()] for line in documents]
	
	dictionary = corpora.Dictionary(texts)
	```
* The word map of the dictionary can be seen by:

	```python
	dictionary.token2id
	```
	
* Gensim uses the dictionary to create a bag-of-words corpus where the words in the documents are replaced with its respective id provided by the dictionary

* Additional words from another document can be added to the dictionary by:
	
	```python
	document2 = [another list of strings]
	
	texts2 = [[text for text in line] for line in document2]
	
	dictionary.add_documents(texts2)
	```
	
* Line-by-line processing to create a dictionary can be done by:

	```python
	from gensim.utils import  simple_preprocess
	
	dictionary = corpora.Dictionary(simple_process(line, deacc=True) for line in open('somefile.txt', 'r'))
	```
	
* A bag-of-words corpus can be created in the following way:

	```python
	doc = [list of strings]
	
	tokenized_list = [simple_preprocess(doc) for line in doc]
	
	mydict = corpora.Dictionary()
	mycorpus = [mydict.doc2bow(word, allow_update = True) for word in tokenized_list]
	```
	
* A dictionary and a corpus can be saved in the following way:

	```python
	mydict.save(filename)
	corpora.MmCorpus.serialize(filename, corpusname)
	```
	
* The TF-IDF scores can be found with:
	
	```python
	from gensim import models
	
	doc = [list of strings]
	
	mydict = corpora.Dictionary(simple_preprocess(line) for line in doc)
	corpus = mydicy.doc2bow(simple_preprocess(line) for line in doc)
	
	tfidf = models.TfidfModel(corpus, smartirs='ntc')
	```
	
* For lemmatization, the following can be used:

	```python
	from gensim.utils import lemmatize
	for word in documents:
		lemmatized_word = lemmatize(word, allowed_tags=re.compile('(NN|JJ|RB)'))
		lemmatized_word = lemmatized_word[0].split(b'/')[0]
	```

* word2vec can be implemented in the following way:

	```python
	from gensim.models import Word2Vec
	
	model = Word2Vec(sentences = list/text/iterable, vector_size = 100, window = windowsize, min_count = 1, workers = 4)
	
	model.train([[...,...]], total_examples = 1, epochs = 1)
	vector = model.wv #Gives the embedding vectors
	```
