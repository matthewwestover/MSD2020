## Week 12 - Day 1
### 3D Rendering
Fundamental Blocks of 3D Rendering:  
Geometry  
Transformations  
Shading

### Geometry
Geometry is "what" you're drawing  
Typically it's the "shape" of the surface of objects ("volume rendering" is its own field which we'll ignore today)  
Many (most?) applications deal with geometry represented by a triangle mesh (some applications use quad meshes, but we'll ignore those for today)  
A triangle mesh is just bunch of triangles

### Triangle meshes
Why triangles?  
It's always possible (and relatively easy) to represent a surface as a triangle mesh (not true for quads or other shapes!)  
triangles are always flat (not true for quads!)  
triangles are the "simplest" 2D shape How should we store a triangle mesh?  
store 3D coordinates for each vertex of each triangle?

### Indexed mesh representation
We can use less space...  
Most vertices are part of 6 triangles  
Store the vertices in an array, then store array indices for the triangles instead This also tells us topology information (which triangles is a vertex part of

### Normal vectors
Another piece of geometric data we need is "normal vectors"  
A normal vector points perpendicularly out from a surface  
For a triangle, it's easy to compute (because a triangle is flat)  
For a mesh, we usually store normals at vertices which are computed by averaging the normals from the triangles adjacent to that vertex  
normal vectors are used for shading, which we'll discuss later

### Transformations
Transformations put our objects into the world  
Modelers work in "modeling" coordinates, which are basically arbitrary  
When we place objects into scenes, we'll need to "transform" them into "world coordinates"   
These transformations are combinations of:

* scale (grow/shrink) 
* translate
* rotate

We can efficiently represent and combine transformations and apply them to vertices  
Basically all transformations are 4x4 matrices which we can multiply together

### Shading
Shading describes how light interacts with our objects There's a few pieces involved with computing this:  
A function that tells us how light that hits a surfaces is emitted (does it change color, which directions does it go in? etc)  
A way to figure out what the "incoming" light is at a point on the surface For each there is a big spectrum that goes from "accurate" to “fast"

### Surface material models
We'll look at a few different surface models:  
uniform (not physical) -- the color of the object is always the same, regardless of where lights are or where the viewer is  
diffuse (models "matte" materials) -- the color depends only on the angles between the light source and the material  
phong (coarse approximation of "plastic" like materials) -- color depends on angles between surface and light AND surface and viewer  
physically based -- there's a lot of these of varying complexity

### Diffuse shading
For diffuse materials, if incoming light is perpendicular to the surface, the surface will shine brighter than if it comes in at an angle  
The position of the viewer doesn't matter -- diffuse materials scatter light equally in all directions  
Diffuse shading gives us some visual cues about the shape of an object

### Phong Shading
A perfect mirror sends all incoming light in the "reflected" direction Phong shading models an "imperfect" mirror  
The closer the viewer is to being in the reflected direction, the more reflected light they see  
As they move away from the reflected direction, the amount of light they see decreases quickly  
Phong shading usually shows us bright highlights which give us more shape cues, especially if the object/light source are moving

### Texturing
When describing a shading material, we have to specify some parameters  
Usually these parameters include color information  
"Texturing" allows this information to vary across our object  
Basically, we wrap an image around our object and use the image colors as inputs to our shading computation  
To do texturing, we need to attach "texture coordinates" to our triangles, basically describing which part of our texture image should we look at when we shade each triangle

### Lighting
For real time graphics, we usually have a small number of "point" lights To shade, we add up the contribution from all the lights in the scene  
Determining if a light is visible from a given surface point is more complex/ expensive (shadowing costs computation)  
Including light reflected from another surface is also often too expensive/ complex to do for real time graphics  
Carefully including all light contributions is called "global illumination." Ray tracing is often used for doing GI rendering

### Cameras and Perspective Projection
To actually SEE our 3D rendering, we take a picture with a virtual camera We use transformations to place our camera in a scene  
Most graphics libraries include a "look at" command which lets us specify the camera position, the point it should be focused on, and which way is up, and gives us an appropriate transformation matrix  
The last piece we need is a way to convert 3D information about the scene into 2D information which is called "projection"  
Projection is a special case of dimensionality reduction

### Projection
One way to do projection is to just throw away depth information  
That's called "orthographic projection" and is useful for some modeling tasks, but doesn't match the way an eye or a camera works  
In the real world, things that are farther away are smaller than things that are close
In perspective projection, we basically divide the size of objects by their depth in the scene to shrink them appropriately