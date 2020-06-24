## Week 1 - Day 3
### Visualization
Easy way to display lots of data really quickly  
Easier to follow than just a table of values  

Visualization is the process that **transforms** (abstract) data into **interactive graphical representations** for the purpose of **exploration, confirmation, or presentation**.  
Communicate and Explore(learn more from hard to understand data) data with others  
Statistics provide a summary, so necessarily discard information   
Visualizations can present much more information  
Leveraging human capabilities

* Pattern Discovery: clusters, outliers, trends
* Contextual Knowledge: expectations for dataset, explanations for patterns
* Action: humans learn and take action
* But: we also have to design for Humans and their limitations

Human visual system is high-bandwidth channel to brain - overview possible due to background processing  
subjective experience of seeing everything simultaneously  
significant processing occurs in parallel and pre-attentively  
Other human senses are less useful for this.  
Just because it can be drawn doesn’t mean it can be read  
There are limits to human perception

**Marks** - geometric primitives - Points, Lines, Areas  
Channels -control appearance of marks - Position (vertical, horizontal, both), Color, Shape, Tilt, Size  
Can redundantly code with multiple channels

**Magnitude Channels** - Ordinal & Quantitative Data  
How much?  
Position Length Saturation

In order of how well Humans can interpret the visualization:

1. Position on common scale 
2. Position on unaligned scale 
3. Length (1D size)
4. Tilt/angle
5. Area (2D size) 
6. Depth (3D position) 
7. Color luminance 
8. Color saturation 
9. Curvature
10. Volume (3D size)

**Identity Channels** - Categorical Data  
What? Where? Shape Color (hue) Spatial region


In order of how well Humans can interpret the visualization:

1. Spatial region
2. Color hue 
3. Motion 
4. Shape

**Color**  
Good for qualitative data (identity channel)   
Limited number of classes/length (~7-10!)  
Does not work for quantitative data!  
Lots of pitfalls! Be careful!  
Minimize color use for encoding data use for brushing  

**Colormap**  
specifies a mapping between color and values  
categorical vs ordered  
sequential vs diverging  
segmented vs continuous  
univariate vs bivariate  
expressiveness: match colormap to attribute characteristics!

**Color Blindness**  
10% of males, 1% of females (probably due to X- chromosomal recessive inheritance)  
Most common: red-green weakness / blindness  
Reason: lack of medium or long wavelength receptors, or altered spectral sensitivity (most common: green shift)

Perception is also different. Humans perceive saturation over sensitively but brightness under  
Other issues with accuracy are: Alignment, Distractors, Distance, Common scale

Rule #1: Use the Best Visual Channel Available for the Most Important Aspect of your Data (Typically position)
Rule #2: The visualization should show all of the data, and only the data

**Tufte’s Integrity Principles**  
Show data variation, not design variation  
Clear, detailed, and thorough labeling and appropriate scales  
Size of the graphic effect should be directly proportional to the numerical quantities (“lie factor”)

**Lie Factor** -  Size of effect shown in graphic / Size of effect in data
Don’t use pie charts - use bar graphs instead

Python has **MANY** libraries to help with this
MatPlotLib is a clone of MatLab and works really well