## Week 8 - Day 1
### Sensors
#### Gyroscope
The gyroscope sensor measures the rate of rotation of the phone along each axis.  
A very simple and elegant use for the gyroscope is to detect when the phone has moved.  
If the phone is still, the gyroscope returns a rate of rotation that is close to zero along all axes.  
If the phone is picked up or moved, the rates of rotation shoot up.  
We only care about the magnitude of the rates, and if that magnitude has gone up.

Remember, the only thing we have to do is override the onSensorChanged() method.  
We want to detect if the total rate of rotation exceeds some user-defined threshold.  
The gyroscope gives us a vector whose components are the rates of rotation.  
We only care about the magnitude of this vector to see how much the phone is rotating.  
Recall that the magnitude of a vector is the square root of the sum of squares of its components.  
In general, the gyroscope is far too sensitive for most apps, and sucks up a lot of battery.

#### Accelerometer
The accelerometer is ideal for gestures, and for measuring specific types of motion.  
As the name implies, the accelerometer measures acceleration (experienced by the sensor itself).  
Acceleration is a vector, which means that we get 3 values back from the sensor: one along each axis.  
There’s one major issue with the accelerometer precisely because of how it works.  
It also measures acceleration due to gravity, which is always tugging downwards!  
If you want to ignore gravity, you’ll have to subtract out its effects.  
The accelerometer uses far less battery than the gyroscope, but is also far less accurate.

#### Gesture Detection
How would you build in gesture detection in your app?  
In practice, see if Android already has a way to do it, or look for a library.  
For learning, we’ll implement our own shake detection.  
But... what is a shake, exactly?  
It’s a quick move to one side, or to the front/back.  
We’ll need to pick a threshold. A slow move shouldn’t register.  
If we exceed the threshold on at least two axes, that should register as a shake.  
Note that you can train people to learn the right gestures by setting thresholds carefully; this is an open research question at the intersection of design and psychology

#### Step Counter and Detector
With a lot of careful processing, it’s possible to write a good step counter or step detector using the accelerometer.  
Fortunately, Android gives us a way of doing this for free (since Android KitKat).  
Two synthetic sensors: the step counter and the step detector.  
The step detector has lower accuracy, but lower latency, and is a trigger sensor.  
The step counter is higher latency but higher accuracy.