[#](#) NLTK

* Tokenization at a word and sentence level can be done by:

	```python
	
	import nltk
	from nltk import sent_tokenize, word_tokenize
	
	text = "string with sentences and punctutations"
	
	sentences = sent_tokenize(text)
	words = word_tokenize(text)
	```

* Frequency distribution of words can be found by:

	```python
	from nltk.probability import FreqDist
	
	distribution = FreqDist(words)
	```
	
* The most common words from the frequency distribution can be found by:

	```python
	distribution.most_common(numberofwordswanted)
	```
	
* Language-specific stopwords as a list can be found by:

	```python
	from nltk.corpus import stopwords
	stopwords = set(stopwords.words(languagename))
	```
* Word stemming can be done by:

	```python
	from nltk.stem import PorterStemmer
	ps = PortStemmer()
	stemmed_words = []
	for word in filtered_words:
		stemmed_words.append(ps.stem(word))
	```
	
* POS-tags of tokenized works can be found by:

	```python
	nltk.pos_tag(tokens)
	```
	
* Lemmatizing of words using NLTK can be done by:

	```python
	from nltk.stem import WordNetLemmatizer
	lemmatizer=WordNetLemmatizer()
	lemmatizer.lemmatize(word, pos = 'a/v/b/r')
	```
	
* Noun/Phrase Chunking can be done with the help of regex expressions in the following way:

	```python
	from nltk import pos_tag
	from nltk import RegexParser
	text = "text".split()
	tokens_tag = pos_tag(text)
	patterns = """:{regex}"""
	chunker = RegexParser(patterns)
	output = chunker.parse(tokens_tag)
	```

