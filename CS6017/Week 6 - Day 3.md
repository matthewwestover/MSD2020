## Week 6 - Day 3
### Dimensionality Reduction
The "extrinsic" dimension of data (the number of columns in your table) is not necessarily the same as the "intrinsic" dimension (the variation in your data)  
As an example, imagine your data is all points on a straight line  
If you draw that line on a piece of paper, the extrinsic dimension is 2 (the paper is 2D), but the intrinsic dimension is 1 (distance along the line). 
If you lift that paper into the air and rotate it, the extrinsic dimension is 3 (points have x, y, z coordinates), but the intrinsic dimension is still just 1  
Dimensionality reductions techniques attempt to convert the data from its extrinsic representation to a lower dimensional intrinsic representation. 
We'll look at one of the simplest DR techniques: Principal Component Analysis

#### Why DR
DR can help many different aspects of data analysis and visualization  
If we can reduce our data to 2 or 3 dimensions, we can actually visualize it!  There's no way to visualize 10D data effectively  
DR can be used to compress data. If we only have to store 2 columns instead of 10, we can fit more data on a phone or other limited device  
If we use compressed data when training other models, they might be faster/smaller as well. For KNN, we can limit the curse of dimensionality. For SVMs we can reduce the cost of training  
DR can help us learn about our data? What is its intrinsic dimension? What are the most important feature combinations?

#### Principal Component Analysis
PCA is a technique for DR that works by rotating the axes so that it aligns with the data as well as possible  
The rotated x axis will be pointed along the direction with the most variation in the data  
The rotated y axis will be pointed along the direction with the 2nd most variation, z axis 3rd most, etc  
We convert our coordinates from the original x,y,z to the new rotated x', y', z' axes  
Since all we did was rotate the axes, the origin stays the same (0 is still 0) and the new axes are still perpendicular to each other 

If we just rotate the axes, our points are still the same dimension, they just have different values in their coordinates... So what?  
Our new axes were carefully chosen so that the new "x" coordinate contains the most important information possible  
The new "y" coordinate contains the most important information not described by x, and so on.  
To do DR, we only keep the first few coordinates of our rotated points. The values in the "z" and further axes will (hopefully) be close to 0 so we just throw them away and assume that they're exactly 0  
For our line example, we'll rotate our axes so the x coordinate goes along the line. Then, for each point, the y' and z' coordinates will be 0, so we just don't store them!

#### Math
The original axes are really vectors:  
`e0 = (1, 0, 0, ...) e1 = (0, 1, 0, 0 ...) e2 = (0, 0, 1, 0, 0, ...)`

So a point p = (x, y, z) can be written as `x*e0 + y*e1 + z*e2`  
We want to find new vectors `w0, w1, w2` and new coordinates, `a, b, c` so that `p = a*w0 + b*w2 + c*w2`  
If we know the w vectors, a, b, c are easy to compute, they're just 
`a = p.dot(w0), b = p.dot(w1), c = p.dot(w2)`

It's also easy to undo the transformation by just adding up  
`p = a*w0 + b*w2 + c*w2`

We want w0 to be the direction that captures the most variation in the data We can "project" each point to w0 by computing `t_i = p.dot(w0)`  
Then, we solve for w0 that produces a set of t's with the largest possible variance. This can be done efficiently  
For the rest of the w's, we repeat the process while making sure they are perpendicular to the w's we've already found

To actually reduce the dimension, we choose how many of the w vectors to use, and how many to throw away  
Remember, `p = r_0*w_0 + r_1* w_1 + r_2*w_2 ...` where each "r" is one coordinate of p.  
To decide what to throw away, we just look at the sum up `(r_i)^2` for all the points which tells us how much information we'll be losing by not including that coordinate  
Because of how we picked our Ws, the `r_0s > the r_1s > the r_2s ...`, so we just go in order until we find one that's "small enough" and then drop the rest of them