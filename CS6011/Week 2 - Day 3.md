## Week 2 - Day 3
### Abstract Classes

*Interfaces* are not a class  
No Implementation  
No member variables  
Classes contain these things but we would also like to leave the implementation of some methods empty  

Abstract classes are middle ground  

Motorvehicle class. We have related classes like cars, motorcycles, jet-skies etc. 

```
Class motorvehicle{
    Private engine
    Protected Motorvehicle(engine e){
    This.engine = e;
    }
    Public void drive(){} // don’t know how to drive as all related classes are super different
}
```

Classes that inherit from this class can fill in the drive method   
Drive method in parent motorvehicle we change to ```public abstract void drive();```  
Class becomes ```public abstract class motorvehicle{}```  
This says that anything that claims to be a motovehicle has to have a drive, like it was claiming it was interface  
It will fill in the drive method uniquely  
This means motorvehicle can have implementation of methods that are true for all children classes, but leave some blank that will differ between children.

```New motorvehicle``` becomes illegal. Abstract classes cannot be new. To create a new one we would have to use a child class.  

Because ```drive()``` is abstract, we can have a list that always calls the correct drive method

```
ArrayList<motorvehicle> vehicles;
For( var v: vehicles){
    v.drive(); // v can be different types and still work
}
```
