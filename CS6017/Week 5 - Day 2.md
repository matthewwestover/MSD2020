## Week 5 - Day 2
### Support Vector Machines
SVMs are another approach for classification. 

#### Separating Hyperplanes
If our data is "linearly separable" it means we can stick a "hyperplane" in between the two classes (all the points above the plane have the same class, all the points below have a different class)

#### Hyperplanes
A hyperplane is the generalization of a plane to arbitrary dimension. In 2D, a hyperplane is a line, in 3D it is a plane. In 4D you can't really picture what it is, but it's "flat.”  
For 2 class classification, defines a linear (straight line/plane) decision boundary A "hyperplane" is the extension of a straight line or plane into many dimensions  
Hyperplanes can be defined by the equation:
`<x, n> + b = 0`

The angle brackets mean "inner product" or "dot product" which is just sum(xi * ni).  
`x0*y0 + x1*y1 + x2*y2 + ...`

The n is the "normal vector" which is a length 1 vector that points perpendicular to the plane. The b basically controls how far away from the origin the plane is.  
When n is a unit vector, <x, n> indicates how far x reaches in the direction of n.  
Since the equation above is 0 for points ON the plane, points with `<x, n> + b < 0` are on one side, and points with `<x,n> + b > 0` are on the other.   
We can measure how far a point is away from the plane by computing `|<x, n> + b|1`.

#### Maximum Margin Hyperplane
If our points in the 2 classes we care about can be separated by a hyperplane, they can probably be separated by an infinite number of hyperplanes (find one, then rotate or translate it slightly, and it will probably still separate the classes).  
We usually want the one with "maximum margin." The Margin is the distance from the plane to the closest point in the data set.  
A point with "minimum margin" is called a "support vector." If you imagine the plane falling under gravity, the support vectors are the ones that keep it in place, by repelling it.  
If the classes are linearly separable, we can efficiently find the maximum margin separating hyperplane.

#### Support Vector Classifier
What if our data is NOT linearly separable? No matter what hyperplane we pick, some points will be on the wrong side.  
In this case, we pick a "margin distance," M, for the plane. We want to minimize the number of points on the wrong side of the margin. If a point is less than M on the wrong side of the margin, it's on the right side of the hyperplane. If it's more than M on the wrong side of the margin, it's on the wrong side of the hyperplane (and would be misclassified).  
Instead of picking M as a parameter, we pick a "budget" which we call C. Each point that is on the correct side of the margin has no cost, and so is ignored when we try to find the best hyperplane. Otherwise, a point's cost is how far it is on the wrong side of the margin. If we pick a small C, then our margin will be very small, which means that most points in the dataset won't be considered while fitting the plane. This means our hyperplane will be very sensitive to the data!  
If we pick a big C, we can have more points in the margin zone, so we consider more points, and our hyperplane will have lower variance.  
We can pick good values of C using cross validation.

#### Support Vector Machines
Basically, solving for the best hyperplane only requires us to compute dot products between vectors (points). If we make up a new definition for dot products, we can fit non-flat decision boundaries.  
A common choice is to redefine the dot product as a "radial basis function: "  
`K(xa, xb) = exp(-gamma sum( (xai - abi)^2 ) )`  
This allows us to fit decision boundaries that are NOT linear.  
There's a bunch of kernels to choose from with all sorts of parameters.