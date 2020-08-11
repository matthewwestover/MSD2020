## Week 8 - Day 3
### Autoencoders
Unsupervised learning with NNs? Today we'll look at how to do (basically) dimensionality reduction with neural nets  
But, how? DR is an "unsupervised learning" technique, but training neural nets (using backpropagation/gradient descent) requires supervision  
We basically have to train neural networks by having some x, y, and trying to find f(x) = y. 
In this case we'll use the dumbest/simplest x/y and train a neural network to be f(x) = x
We call these autoencoders (auto meaning "self”)

#### DR with autoencoders
Autoencoders are "hourglass" shaped  
The narrow, middle section is the "encoded" or "reduced" representation  
The left half of the network is called the "encoder" which takes the data as input and produces the encoded representation as output  
The right half is the "decoder" and recovers the full representation from the reduced representation (probably with some error)  
Often the two halves are symmetric, but they don't have to be!

#### Other uses of autoencoders
Generate new data -- These networks are called "generative autoencoders” - the idea is that the user provides a reduced input (maybe chosen randomly) and the network produces an output which is similar to the training data  
Anomaly detection  
For inputs that are similar to the training data, the "reconstruction error" should be small... that's how we trained the network  
But for data that's NOT like the training data, the reconstruction error might be large  
A student used this idea for his capstone this year. He needed to detect anomalous network traffic, but didn't have any to train on. He classified points as "normal" or "suspicious" by looking at the reconstruction error when running them through an autoencoder

#### Combining with CNNs
If we're working with images, we might want to use convolution/max pooling layers in our autoencoders  
Convolution layers work without any special care, but pooling is tricky  
The inverse of a pooling layer is an "upsampling" layer which takes 1 pixel and turns into a 2x2 (for example) block of pixels  
There's a few different varieties of these we can play with. "Bilinear" is similar to how "good" image editing software would do upscaling, and "nearest" is how "bad" image editing software would work  
In any case, we'll want layers AFTER the upscaler to try to fix the issues caused by upscaling


