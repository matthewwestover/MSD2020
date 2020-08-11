## Week 8 - Day 1
### Neural Networks
Neural nets are a machine learning technique based on a very coarse and inaccurate model of the human brain  
They are very powerful because they contain hundreds/thousands/millions of parameters which can be trained  
For least squares, we have a few betas to train (<10 probably)  
For SVMs, we have the hyperplane normal vector to train (#dimensions parameters)  
The types of functions we can fit with these is somewhat limited because we have a relatively small number of parameters  
With a neural net, we can (theoretically) fit almost any function

#### The Perceptron
A perceptron is a multi-input single output function/node  
It start out by computing a linear combination of its inputs along with a "bias”:  
`y = b + w1*x1 + w2*x2 + w3*x3 + w4*x4 + ...`  
The x's are its inputs and the b is the "bias." You can also think about the bias as a weight for a special "input" whose value is always 1  
So far this looks exactly like linear regression... because it is
We can hook up as many of these perceptrons as we want, and at the end what we get is a complicated way of writing out linear regression...  
The secret to model more complex functions with perceptrons is to "smush" the output

#### Activation Functions
So, we take the linear combination of inputs and run it through an "activation function" which "smushes" the output somehow. If we do this for all of our perceptrons, the "network" of them can model all sorts of crazy functions  
The activation function introduces "non-linearity" which gives us modeling power Some common activation functions are:  
sigmoid or logistic function or "tanh" -- we've seen this smushes things into a range of 0->1 (logistic) or -1->1 (tanh) with a sort of "S" shape  
"step" function -- input <= 0 ? output 0. input > 0? output 1  
"relu" -- input <= 0? output 0 else output the input. This seems to be the most popular function these days 

#### Neural Nets
Last slide had a "feed forward" neural net which had no "back edges" Each column of nodes is called a "layer"  
We'll look at a few types of layers going forward, but we'll start with "dense"layers  
A "dense layer" connects all the nodes of the previous layer as inputs, and is used as an input to every node in the next layer  
The first and last layers are the "input" and "output" layers The rest are called "hidden” layers

#### Topology
How many hidden layers should we have? How big should they be?  
Designing the topology of a network is a difficult problem. AFAICT a significant amount of intuition and witchcraft are still common  
Historically, people spent a lot of time on the input layer designing "features" by hand. For example, if the data has x, and y, attributes, you might include nodes in the input layer for x^2 and y^2.  
Modern neural nets are "deep." The input layer is typically just the raw data, and hidden layers are expected to "learn" that "x^2" or "y^2" are important  
Edge edge an a NN adds more complexity to the models we can fit. This allows us match complicated "shapes" of data, but means we will have more parameters that we need to train. We're often trading off "trainability" and “power"

#### Training
"Training" a neural network means finding values for all the w's and b's
The general family of algorithms for training is called "back propagation"  
We start by choosing random values for all the weights  
Then, we feed a few examples through the network and compare the output to what we expect and compute the error (NN folks call this "loss")  
Using calculus, we compute the "gradient" of the loss with respect to the weights. In English, the gradient tells us how to tweak the weights slightly to decrease the loss slightly  
We repeat this process until we run out of time, or we think that we've found an optimal set of weights.  
The family of algorithms that work this way are called "gradient descent”

#### Gradient Descent
Gradient descent is basically like "rolling a ball down a hill." If we have 2 weights, we can think of them as our x/y position, and the loss is our elevation  
The gradient tells us which direction is the steepest "downhill" from where we are (it really points uphill, so actually -gradient is downhill... whatever)  
We can't just go in that direction forever because the landscape can be very rough. But, if we follow the gradient for a couple of steps, we probably went down a little bit  
We can add "momentum" so that we follow the gradient a little bit, but also keep "rolling" in the direction we were moving before  
Hopefully we'll get to the absolute lowest spot on the map and won't get stuck in a high valley, but gradient descent methods don't provide any guarantees  
The "smoother" the hillside, the easier it is to roll to the bottom. If the hill is super jagged, it's easy to get stuck. If the hill has big flat spots, it's hard to roll toward the bottom. We want out "loss function landscape" to be as smooth as possible