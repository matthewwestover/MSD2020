## Week 8 - Day 2
### Convolutional Neural Nets
Neural Nets are commonly used for image analysis tasks, such as classification.  
Imagine we have a ~small image: 1 megapixel, color image. That means we have ~3 million input features  
This is a pretty small image, but if we were to add a dense layer of the same size, it would have 9 trillion edges... That's insane - Terabytes worth of data for a single layer in the net  
That's too many weights to train (or even store), so we somehow need to reduce the number of weights to train while still being able to operate on an image like this  
The trick is to use a "convolution layer" which is inspired by "old school" computer vision techniques

#### Convolution
Convolution is a calculus thing that basically allows us to quantify how much each small patch of an image matches a given pattern  
We need a "convolution filter" which is a small pattern (maybe 3x3 or 5x5 pixels)  
We drag this filter across the whole image, and for each little block we multiply the corresponding pixel values of the image pixel and filter pixel, then sum them up  
This gives us a new "image" where each new pixel tells us how strongly the original image patch matches the pattern

#### Old school convolution
People used to design convolution filters to pick out features of images These filters could detect things like edges or corners  
This is a bit like the example we saw with the tensorflow playground where we added input nodes for x^2 and y^2  
This is also a bit like we discussed with NLP where people came up with clever solutions to solve specific NLP tasks  
Now that we have neural nets, let's just let them learn what a good convolution filter is

#### Convolution Layer
A convolution layer is equivalent to several of the "old school" convolution filters, except that the pixel pattern is determined by the network weights, not designed by hand  
We specify how many of these filters we want (# of output channels) and the filter size (3x3, 5x5, 8x8 pixels, etc)  
We apply each of these filters to every patch on the original image, so the output from this layer is basically #outputs images  
Assuming I have a 100x100 grayscale image as input to the layer, and I say I want 8 filters, this layer output would be 8x100x100

If my output is way bigger than my input, why is this better than just using a dense layer?  
We're actually training WAY WAY WAY fewer weights, because the SAME filters are used across the whole image  
We only have to train #filters*filterSize weights for this layer!  
The other advantage of convolution layers is that they consider which pixels are next to each other. As far as a dense layer is concerned, everything is just independent numbers. But for a convolution layer, we look at small patches of neighboring pixels.

Removing more weights: **Max Pooling**  
If use a lot of filters, we'll have big networks with lots of weights in future layers.  
We can reduce the number of weights by "downsampling" the output of one layer  
A common approach is "max pooling" where we take a small block of pixels (say 2x2) and just output the max value.  
Applying a 2x2 MaxPool layer reduces the size of the input by a factor of 4  
In CNNs, we often pair convolution and max pooling layers: do a convolution, then max pool it, add another convolution layer, then max pool it, etc.

#### Classification
Perceptrons, as we've described them output a continuous value: activation(b + w1*x1 + w2*x2 + ...)  
How do we use the value we get out of a neuron in our output layer for prediction?  
We could say output from 0->.999 means label 1, 1.0 -> 1.9999 means label 2, etc?  
This probably wouldn't work very well...  
Usually we have an output neuron for each label. The one with the highest output is our prediction.  
Typically we use the "softmax" function to normalize the outputs to represent probabilities. The probability of a given label is exp(label)/sum(all labels: exp(label))  
If the largest labels' probability isn't close to 1, we can interpret this as the network being "unsure" or "unconfident" about its prediction
