## Week 9 - Day 3
### Neural Networks for Natural Language Processing
We talked a few weeks ago about how one popular approach to NLP is to turn text into numbers (somehow) and then use our normal techniques (classifiers, etc) to solve our problems  
We looked at one technique for converting a document into numbers, the bag of words model  
Today we'll look at a couple techniques that use neural nets to do this and play with an example

#### Embedding Layer
Today we'll use pretty much the simplest possible way to turn words (words, instead of documents this time) into points/vectors  
Basically, we'll just have a big table with one row for each word in our "language" That row just stores the "coordinates" of that word in our "embedding" space.  
So, if I decide that my words will be 100 dimensional points, and I have 5000 words, I'll just have a big table with 5000 rows and 100 columns  
An embedding layer takes in a "word index" (which row to look at) and just outputs the contents of that row... pretty simple. 
The big question is of course, how do we fill in the table?

#### Answer: Gradient Descent
It doesn't really make sense to train the embedding on it's own... A good embedding will depend on what it's used for after the conversion  
So basically, we'll have an embedding layer as our input layer (or right after our input layer), the we'll have the rest of network after it which performs some task (classification maybe)  
Then, we can train our network, including the entries in our embedding table, with gradient descent like we've been doing  
If we look at the entries in the table after the fact we can some understanding of the "geometry" of the embedding space and ask questions like "which words are similar”

#### N-Gram Language Models
We'll play with a simple "classifier" based language model.  
An N-Gram is a sequence of N consecutive words in a document  
We'll train a neural network to take in N - 1 words and predict the next word  
This doesn't really sound like classification, but it is because we can think of each word in our "language" as a discrete label. So, we'll have 1 node in our output layer for each word in our language and use something like softmax to perform prediction  
Our network will be an embedding layer, followed by some dense layers, and finally an output layer of size (# of words in the language)  
These are easy to get training data for... we just look at all the N word sequences in our document

#### Other language models
The N-gram model really only lets us predict the "next" word based on the previous model, but there are other ways of incorporating the "context" into our model  
If we consider the words on both sides of the word we're trying to predict that's called a "skipgram model"  
These other models more or less just change our training data set, but the process is generally the same: we input some number of words, and we get a predicted word as output