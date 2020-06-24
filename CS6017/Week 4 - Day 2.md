## Week 4 - Day 2
### Psuedocode for KNN Structures
```
struct BucketKNN:
    #member vars
    #figuring out which bucket a point is in
    number of splits
    coordinates of min corner
    size of the grid cells
    #the actual buckets
    buckets (bucket[] or hashtable) 

    constructor(points, optional(number of splits)):
        #number of splits = function(points.size) maybe sqrt? log?
        aabb = bounds(points)
        min corner = aabb.min
        cell size = (aabb.max - aabb.min)/num splits
        initialize the buckets array

        for each p in points:
            add p to buckets[getBucketIndex(getBucketCoords(p))] 

    rangeQuery(p, radius):
        minSearchArea = p - radius
        maxSearchArea = p + radius
        minBucket = getBucketCoords(minSearchArea)
        maxBucket = getBucketCoords(maxSearchArea)

        for each bucket between min and max:
            for each point in that bucket:
                if it is closer than r to p:
                    add to output

    KNNQuery(p, int k):
        r = bucket size?
        rangeQuery(p, r)
        if I didnt get at least k neighbors:
            increase r += ? or r *= 1.5? 2? , try again
        #at least K neighbors, probably more
        sort and keep the closest k (could use "Top N"/"order statistic") 

    getBucketCoords(p): 
        coords = (p - min corner)/cell size
        #cast to ints, clamp to (0, numSplits -1)

    getBucketIndex(coords):
        index = x*numSplits + y

        row major:
            0 1 2 3 4
            5 6 7 8 9

        column major:
            0 2 4 6 8
            1 3 5 7 9

        index = sum coord[i]*numSplits^(dimension - i) 

        alternative: use some hash function

    bucketsBetween(min, max):
        min = 1, 2, 3
        max = 2, 4, 5
        buckets I want:
            1, 2, 3
            1, 2, 4
            1, 2, 5
            1, 3, 3
            1, 3, 4
            1, 3, 5
            1, 4, 3
            1, 4, 4
            1, 4, 5
            2, 2, 3
            2, 2, 4

        #no go if dimensions isn't known

        for x = 1..2: for y = 2..4: for z = 3..5: get x,y,z

        "add with carry" to get this sequence also "cartesian product"

struct KD Tree: 
    #member variables
        node root

    class node:
        splitLevel (x split? y split? z split? ...)
        p (split point)
        node left, right

        constructor(points, level):
            "sort" points by level coordinate (x, y, z, etc)
            p = middle point
            left = new point( left half , next level)
            right = new point( right half, next Level) 

    constructor(points):
        root = node(points, 0) 

    rangeQuery(n, p, r): 
        if(n.p is closer than r to p)
            add it to the output
        #if the left edge of my range overlaps the left subtree
        if p[SplitLevel] - r <= n.p[splitLevel]
            rangeQuery(n.left, p, r)
        if p[SplitLevel] + r >= n.p[splitLevel]
            rangeQuery(n.right, p, r) 

    knnQuery(p, k):
        knnQuery(root, p, k, infinite bounding box)
        knnQuery(n, p, k, aabb):
        shared output "list" (priority queue/max heap) 

        look at n.p and output if list "has room".
        or If, closer than the "farthest" point in list, replace that one
        left child box = parent box, chopping down one side (splitDimension max)
        find the point in child box that is closest to p

        if list has room or closest in box is closer than the farthest in list:
            recurse
        similar for right child


Struct QuadTree:
    Node root

    class Node:
        bool isLeaf (bucket or internal node)
        Point[] bucket
        Node ne, nw, se, sw (children)
        AABB bounds # what area does this node cover

        constructor(points, aabb):
            if points.size < threshold
                # leaf node
                bucket = points
            else
                # internal node
                splitLines = average(bounding box)
                left/right half = partition by x based on splitline
                partition each half by y coordinate
                #top left, for example
                top + left bounds are bounds of the parent
                bottom + right bounds are the split lines
                topLeftChild = node(top Left points, shrunken bounding box)
                #similar for the other 3 children

    constructor:
        root = node(points, bounds(points) ) 

    rangeQuery(n, p, r):
        if leaf:
            loop through bucket, and keep the ones closer than r to p
        else (internal node)
            #top left, similar for other children
            #consider (px - r, px + r). If it's in TL's bb, then recurse
            find the point in TL box that is closest to p
            if its closer than r, recurse
            closest = clamp(p, aaabb min/max) in each dimension

    knnQuery(n, p, k): 
        shared output "list" (priority queue/max heap)
        if leaf:
            loop through bucket, and output if list "has room".
            If, closer than the "farthest" point in list, replace that one
        else internal node:
            find the point in TL box that is closest to p
            if list has room or closest in box is closer than the farthest in list:
            recurse
```