## Week 7 - Day 2
### Natural Language Processing

We've discussed how to work with categorical and continuous data... but what about text?  
We'll look at some techniques for turning text into high dimensional points
If we do that, we can apply any of the techniques we've looked at to perform classification, clustering, etc  
We'll also look at some tools for more exploratory analysis  
We'll make use of the "natural language toolkit" NLTK which has been a popular python library for a long time  
Language is developed over a lifetime. Hard to transfer to a computer. 
We take “text” and turn them into points then do analysis

#### Basic Text Analysis
A couple of tools useful for analysis:

* "concordance" -- the context around a given word (a few words on either side)
* "similar" words appear in the same context
* "stop words" are "filler" words that don't add meaning like "the" or "and" "collocations" are pairs of words that show up together frequently

#### Statistical measures
* Frequency distribution -- count how many times each word appears
* "Dispersion plot" -- basically a scatterplot of the occurrences of a word. If the text covers a large period of time (maybe a log file?) you can see how the frequency of words has changed over time
* "N-grams" -- rather than do statistics on single words, perform them on a sequence of words (looking at the frequency of ["not, "bad"] tells you something different than the frequency of "bad")
* Google's "ngram" viewer is useful for viewing frequencies of ngrams over time

Since we already have a lot of tools for classification, it would be nice if we could apply to them to text  
One approach is to convert a "document" into a "vector" and then use our normal tools on that vector  
There are many ways to do this, but we'll look at an extremely simple model called the "bag of words” model

#### Bag of Words
First, we decide on our "features" which is a set of words from our corpus of documents (from all of our training text)  
One approach is to take the most common words (removing stop words) in the collection and use these. Say we have M features  
We turn each document into an M-dimensional vector by setting the i'th entry to 1 if the document contains the i'th feature word, and 0 if it doesn't  
It seems a little strange to just store present or not rather than the number of occurrences, but the binary value works well in practice  
Now we have a set of points, so we can do KNN, SVMs, logistic regression, PCA, clustering... everything!

#### Word2Vec
Word2Vec is a similar approach but it turns words into points instead of documents into points  
After running word2vec on a corpus, words which are similar are represented as points that are close together!  
There are a TON of approaches for NLP that make use of neural nets
Other tasks include:

* part of speech tagging -- is a given word a noun, verb, subject, object, etc 
parsing -- build a "sentence diagram" like you might have done in Jr. high 
* machine translation
* automatic summarization
* pronoun disambiguation -- who is "he" and what is "it?"

Historically, there were lots of special purpose algorithms/techniques for doing these things. Recently, using machine learning with a lot of training data is very popular

