## Week 9 - Day 1
### Image Compression and Face Detection
#### Linear Algebra 
A scalar is a number.  
A (column) vector is a column of one or more numbers. Recall vectors have magnitude and direction.  
A matrix is an object whose columns are vectors.  
We have an intuition on what a vector is: it’s something that points in some direction and has a magnitude.  
There are two ways to understand a matrix.  
The first way requires us to think about whether the columns of the matrix mean something when taken together.  
Its columns are the unit vectors pointing along the x-axis, y- axis, and z-axis.

The second way is to understand a matrix based on what it does to a vector.  
If you multiply a vector with a scalar, it scales (grows/shrinks) the vector’s magnitude.  
There are two ways to multiply 2 vectors in 3D: the dot product, which produces a scalar, and the cross product, which produces a new vector.  

Hit a vector with a matrix, and you get out a new vector!  
This lets us understand matrices as objects that transform vectors.  
More precisely, matrices (in general) take vectors to vectors.  
This limits what a matrix can do to any vector, since a vector has only direction and magnitude.

#### Matrix-Vector Products
Matrices can destroy/annihilate vectors (make magnitude zero).  
Matrices can stretch/shrink (scale) vectors. Matrices can rotate vectors.  
Matrices, depending on dimensions, can also take vectors from lower to higher dimensions.  
Example: multiply a 3x2 matrix with a 2D vector, you will get a 3D vector.  
Every matrix has the potential to do one of these operations!  
Let the rank of the matrix be r (number of independent columns/rows).  

#### The Singular Value Decomposition
Since every matrix has the potential to rotate, scale, and embed, there should be a way to extract out these parts for any matrix.  
This can be done using the SVD: A = USV^T. 
VT is responsible for rotation and moving the vector from an n-dimensional space to an r-dimensional space. It won’t change the magnitude if applied to a vector b.  
S is responsible only for scaling. S only has values on the diagonal.  
U does another set of rotations in the m-dimensional space.  
We changed our matrix into a sum of m terms involving vectors and scalars.  
This suggests a natural way to approximate A: drop a few terms.  
Using fewer terms means needing to store fewer vectors and scalars, which means less memory!  
This is a way to compress A.  
Say we retain only k terms. The resulting matrix Ak is the best approximation to A that can be made using k terms.  
The best “rank-k” approximation to A.

### SVD and Image Compression
We can now use the SVD to compress anything that can be represented as a matrix.  
You can compress a set of vectors (useful for machine learning in high-dimensional spaces).  
Our focus is image compression.  
An image is a grid of pixels with certain colors assigned to each pixel.  
Matrices are a convenient way to store images.

#### Images as Matrices
Consider a grayscale image of size 1920x1080.  
Each pixel in this image has a color, an integer value between 0 and 255.  
You could store this image as a 1920x1080 matrix A, whose entries correspond to pixel colors (a number from 0 to 255).  
In general, images have 3 “channels”: Red, Green, and Blue (RGB). Sometimes an additional channel for transparency (alpha).  
You therefore typically need 3 matrices to represent an image.  
Do the same SVD on each of these matrices, retain some number of terms, assemble back.

#### Face Detection: Algorithms
So, face detection is not entirely foolproof.  
What are the algorithms underlying this approach? There are many, and we’ll cover only one common one.  
The most common one is a combination of two techniques:  
Histogram of Oriented Gradients (HOG).  
Support Vector Machines (SVM).  
First appeared in a paper titled “Histograms of Oriented Gradients for Human Detection” (2005) by Navneet Dalal and Bill Triggs, both from INRIA.

#### HOG-SVM
The idea is that local object shape/appearance can be characterized by distribution of local intensity gradients.  
This means you need to estimate derivatives of the image in both x and y directions.  
Once you have the gradient vector, you can find the angle it makes with the x-axis.  
This is the gradient orientation. Divide the image into non-overlapping 8x8 cells.  
Inside each cell, compute a histogram of gradient orientations. In practice, 9 bins are used.  
Group cells into overlapping blocks of 2x2 cells each. Concatenate the cell histograms in each block into a single block feature vector and make this feature vector unit length (divide out its magnitude).  
Collect all the block feature vectors into a single HOG feature vector, divide out its magnitude, clamp each.  
entry to be no larger than some predetermined threshold (so large gradients don’t have too much influence).  

We need a training set for the SVM.  
Sample P positive samples of faces from the training data, and collect HOG feature vectors for these samples.  
Sample N negative samples from a negative training set that does not contain faces, collect HOG feature vectors. Ensure N>>P.  
Train an SVM on the positive and negative samples using the HOG feature vectors.  
Hope that it works. In practice, this will not work perfectly, and can fail even on uncompressed images.  
It somewhat arbitrarily missed some faces in our image.