## Week 9 - Day 2
### Generative Adversarial Networks
The goal of a GAN is to "generate" new outputs. As an example, we might want to train a network to output images of animals  
We basically want a "generator" network that takes in a low dimension random vector (called a "latent vector") and produces some output (say, an image) that's "like the real thing"  
This seems pretty hard to do because how do we know if our output is "like the real thing"  
Like many problems, the solution to this one is "use a neural network”

#### Generator and Discriminator
G is what we want. It takes a low dimension, probably random vector, and produces an output that's "like" our training data  
We train G with the help of another network, the Discriminator  
D takes in an image and performs binary classification: is this image from our dataset, or not?  
We train both of these networks at the same time, and they help each other out. As D gets's better at telling real an fake images apart, G gets better feedback about whether or not its fakes are convincing!

#### Training Loop
Improve D:  
Feed D a bunch of real images and compute the loss/gradient (the label for all these is "real") Feed D a bunch of images made by G and computes loss/gradient (the label for all these is "fake") Update D's weights based on the gradient from those 2 steps

Improve G:  
Generate some images with G and feed them to D. The expected labels for these are "real" (G wants D to think they're real)  
Compute the loss/gradient (loss is basically how "fake" D thought these images were) Update G's weights based on the gradient

New Layers - These aren't specifically related to GANs, but can be used

#### Batch Normalization
If we update the weights of a layer close to the input, it can have a dramatic result on our final output  
Basically, each subsequent layer can amplify that change  
Mathematically, this means we have very large gradients (our loss landscape is super steep), so it's very difficult to train our network. We have to take very, very small steps in the downhill direction or we may overshoot by a lot and actually increase our loss!  
Batch Normalization is a way to transform the output of a layer to reduce the exploding gradients problem

A batch norm layer takes a batch of inputs (say 64 or 128 training samples), with values x and outputs y:  
`y = gamma * (x - average(x))/sqrt(variance(x) + epsilon) + beta`  
The average and variance are computed from all the values in the batch (each training input will provide one value)  
This basically rescales the output so that it's less dependent on the scale of the input.   This reduces the magnitude of the gradients from the previous layers  
The parameters gamma and beta can be trained as part of our normal training process The epsilon is there to make sure we don't divide by 0

#### Convolution Transpose Layer
This is an "upsampling convolution layer"  
Basically it adds extra rows and columns of zeros throughout the input to control the desired output size, then works the same way as a normal convolution layer
