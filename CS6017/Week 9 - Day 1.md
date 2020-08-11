## Week 9 - Day 1
### Recurrent Neural Nets
All of the neural nets we've discussed so far, while useful, have a major limitation  
They operate on fixed sized data. The input and output layers have a fixed size that we need to choose before training them  
This poses problems for some common tasks  
Speech to word: Longer words will have longer inputs than shorter words (let's assume our output is like classifiers we've discussed, one output for each word that we know)  
Image description: Fixed size input image, but output depends on what the picture is of ("a dog sitting" vs "3 monkeys riding a zebra")  
Machine translation: variable size input string, variable sized output string  
We need to somehow be able to handle inputs and outputs of variable length…

#### Key Idea of RNN
Variable sized data can be thought of as a SEQUENCE of fixed size data A variable length string is a sequence of fixed-sized characters  
A variable length audio clip is a sequence of fixed sized samples  
So, we split our data into fixed sized pieces and run them through our network one at a time

#### Problem
Imagine using one of the networks we've used so far for text prediction  
Our input is the current character, and the output is the predicted next character Let's say we're training our network on the word "hello"  
Input: h, expected output: e  
Input: e, expected output: l  
Input: l, expected output: l  
Input: l, expected output: o  
....uh oh. Our network should produce different outputs for the same input. That doesn't really make sense

#### Solution: Memory
We'll use a new type of node that stores some data internally This data helps the node "remember what it's seen"  
This allows our node to (conceptually) decide whether to output an "l" or "o" depending on whether it saw an "e" or an "l" recently  
Our new type of node will do 2 things in its forward pass: produce an output (like we did already), and update its internal state

#### RNN
The simplest implementation of this idea is to use "recurrent neurons" in a recurrent neural network To step a "normal" neuron, we basically computed output = activate(W * X)  
To step a "recurrent" neuron, we do 2 steps:  
H = activate(W1*H + W2*X) # H stands for "hidden state"  
output = W3*H  
So each neuron now has 3 sets of weights:  
weights for the inputs (like we had before) -- how we remember what we see  
weights for updating the hidden state with itself -- how our memory changes over time  weights for producing output from the hidden state -- how our output depends on our memory

#### Long Short Term Memory
LSTMs are a popular type of recurrent node that have slightly more complicated update rules  
The top line is basically the hidden state memory, and the bottom is a "control" signal for deciding how to remember or forget data  
Again, they work basically the same as a normal recurrent neuron, just with more complicated rules for how to update the hidden state (note, the h here is output, not hidden state for some reason... blame https://colah.github.io/posts/2015-08-Understanding- LSTMs/)

#### Training
Training RNNs can be tricky because we need to train not just the weights, but we also need to make sure our hidden state has appropriate values during training  
One approach is to "unroll" the network a few times and then use gradient descent like we've been doing (but keep in mind that all the "copies" are actually sharing the same weights)

#### RNN “Old"
Things move quickly in the NN world...  
In 2014 RNNs were blowing up and were superpopular  
Now they're being replaced with other techniques/architectures  
One issue reason is that they're difficult to train (how long should training sequences be, what should the hidden state values be?)  
Long training sequences mean our gradients can get really big (a small change in weights can mean a BIG change in the character I predict 100 characters in the future!)  
Generally, people seem to be using 1D convolution layers to work with sequence data these days