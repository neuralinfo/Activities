# Text Processing with NLTK #


## Stopwords ##

Many languages contain words that occur very often (e.g., "the" or "a" in English) and their frequent use will
overwhelm more interesting words useful in analysis.  A common technique is to use a stop word list to exclude
such common words from further processing.

NLTK supports stop words for a number of languages and they are accessed as:

    stopWords = nltk.corpus.stopwords.words('english')
    >>> stopWords
    [u'i', u'me', u'my', u'myself', u'we', u'our', u'ours', u'ourselves', u'you', u'your', u'yours', 
     u'yourself', u'yourselves', u'he', u'him', u'his', u'himself', u'she', u'her', u'hers', u'herself', 
     u'it', u'its', u'itself', u'they', u'them', u'their', u'theirs', u'themselves', u'what', u'which', 
     u'who', u'whom', u'this', u'that', u'these', u'those', u'am', u'is', u'are', u'was', u'were', u'be', 
     u'been', u'being', u'have', u'has', u'had', u'having', u'do', u'does', u'did', u'doing', u'a', u'an', 
     u'the', u'and', u'but', u'if', u'or', u'because', u'as', u'until', u'while', u'of', u'at', u'by', 
     u'for', u'with', u'about', u'against', u'between', u'into', u'through', u'during', u'before', 
     u'after', u'above', u'below', u'to', u'from', u'up', u'down', u'in', u'out', u'on', u'off', u'over', 
     u'under', u'again', u'further', u'then', u'once', u'here', u'there', u'when', u'where', u'why', u'how', 
     u'all', u'any', u'both', u'each', u'few', u'more', u'most', u'other', u'some', u'such', u'no', u'nor', 
     u'not', u'only', u'own', u'same', u'so', u'than', u'too', u'very', u's', u't', u'can', u'will', u'just', 
     u'don', u'should', u'now']
     
The `nltk.corpus.stopwords` module just returns a simple list of words you can use in your own code.  For example,
a simple list comprehension can be used to filter a list of words:

    stopWords = nltk.corpus.stopwords.words('english')
    filtered = [e.lower() for e in words if not e.lower() in stopWords]
    
and another trick is to add your list of punctuation to the stop word list:

    stopWords = nltk.corpus.stopwords.words('english') + ['.',',']
    filtered = [e.lower() for e in words if not e.lower() in stopWords]

The languages supported by NLTK can be discovered by inspecting the `nltk.corpus.stopwords` object:

    >>> nltk.corpus.stopwords
    <WordListCorpusReader in u'/usr/share/nltk_data/corpora/stopwords'>

The reader outputs the directory in which the stop words are stored.  You can list the suppored langauges:

    $ ls /usr/share/nltk_data/corpora/stopwords
    README		english		german		norwegian	spanish
    danish		finnish		hungarian	portuguese	swedish
    dutch		french		italian		russian		turkish
    $ head -n 10 /usr/share/nltk_data/corpora/stopwords/english
    i
    me
    my
    myself
    we
    our
    ours
    ourselves
    you
    your 

The files contain a single word per line.  As such, you can create or modify a stop word list for any language and add it to NLTK.

## Normalizing Text #
## Stemming and Lemminization #

Stemming: the process for reducing inflected (or sometimes derived) words to their stem, base or root form.

Lemmatization: the process of grouping together the different inflected forms of a word so they can be analysed as a single item.

NLTK supports:

  * [Porter Stemming](http://tartarus.org/martin/PorterStemmer/)
  * [Lancaster Stemming](http://www.comp.lancs.ac.uk/computing/research/stemming/)
  * [Snowball Stemming](http://snowball.tartarus.org)
  * Lemminization based on [WordNet’s built-in morphy function](http://wordnet.princeton.edu)

For stemming, you construct a stemmer and then call `stem()` on the word:

    from nltk.stem.lancaster import LancasterStemmer
    stemmer = LancasterStemmer()
    w = lancaster_stemmer.stem(‘presumably’)  # returns u’presum’

In the above, you can use `nltk.stem.porter.PorterStemmer`, `nltk.stem.lancaster.LancasterStemmer`, or `nltk.stem.SnowballStemmer`.

Lemmatization is similar:

    from nltk.stem import WordNetLemmatizer
    lemmatizer = WordNetLemmatizer()
    lemmatizer.lemmatize(‘dogs’)  # returns u'dog'
    
but the lemmatizer assumes by default everything is a noun.  For verbs, this means that results are not lemmatized 
properly (e.g., "are" and "is" do not become "be").

For example, try:

    from nltk.stem import WordNetLemmatizer
    lemmatizer = WordNetLemmatizer()
    lemmatizer.lemmatize('is',pos='v')
    lemmatizer.lemmatize('are',pos='v')
    
The `pos` argument can have the following values:

   * 'a' - adjective
   * 'r' - adverb
   * 'n' - noun
   * 'v' - verb

## Activity ##

Using BeautifulSoup get the text of the Fox News homepage (http://www.foxnews.com):

   1. Tokenize the text.
   2. List all the nouns in the text.
   3. Apply a stop word filter to the tokenized text.
   4. Compute and plot a frequency distribution of the top 50 words.
   5. Apply a lemmatization algorithm with the pos argument set to 'n' and recompute your frequency distribution.
   
