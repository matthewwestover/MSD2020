## Week 2 - Day 1
### How to Design Programs
Determine **representation** - classes if needed  
Write a **purpose statement** - comments  
Write **examples** - tests - write tests for every variant and outcome. Cover edge cases  
Create a **template** - methods, extract field values, cross/self-calls  
**Finish body**  
**Run tests**

#### Example #1 - Cookies 
Representation  
Keep track of cookies in a cookie jar  
Question is not where do I keep the cookie count but what data type lets me represent a cookie count? Ints

**Purpose**

```
// To return the number of cookies left eating one
Int eat_cookie(int n);
```

**Examples**

```
CHECK( eat_cookie(10) == 9 );
CHECK( eat_cookie(1) == 0 );
CHECK( eat_cookie(0) == 0 );
CHECK( eat_cookie(-1) == );
```

These examples may refine purpose or suggest **contracts**  
**Purpose** updates this to 

```
// To return the number of cookies left eating one;
// given number of cookies must be non-negative
Int eat_cookie(int n);

CHECK( eat_cookie(10) == 9 );
CHECK( eat_cookie(1) == 0 );
CHECK( eat_cookie(0) == 0 );
```

**Template**

```
// To return the number of cookies left eating one;
// given number of cookies must be non-negative
Int eat_cookie(int n) {
    …. n …. // Stating we need to use the param n somewhere n here
}
```

**Body** - start with template and pay attention to example test cases to fill in methodology

```
// To return the number of cookies left eating one;
// given number of cookies must be non-negative
Int eat_cookie(int n) {
    If (n > 0)
        return n - 1;
    Else
        return 0;
}
```

#### Example #2 - Position
**Representation**  
Track a position on the screen  
Multiple components to keep track of - make a new class  

```
class Post {
    Int x;
    Int y;

    Posn *flip() { // No params to pass as it does flip on “this” and returns a new Posn
        ...
    }
}
```

**Purpose**

```
class Post {
    Int x;
    Int y;
    
    // To return the opposite post over the diagonal
    Posn *flip() {
        ...
    }
}
```

**Examples**

```
CHECK( (new Posn(1, 17))->flip()->equals(new Posn(17,1)) );
CHECK( (new Posn(-3, 4))->flip()->equals(new Posn(4,-3)) );
```

**Template**

```
class Posn {
    Int x;
    Int y;
    
    // To return the opposite post over the diagonal
    Posn *flip() {
        … x … y … // These are the fields we can access from “this”
    }
}
```

**Body**

```
class Posn {
    Int x;
    Int y;
    
    // To return the opposite post over the diagonal
    Posn *flip() {
       return new Posn(y, x);
    }
}
```

#### Example #3 - Ants
**Representation**  
Track an ant, which has a location and weight  
Complex component - use other class  

```
Class Ant {
    Posn *loc;
    Double weight;

    Bool is_at_home() {

    }
}
```

**Purpose**

```
Class Ant {
    Posn *loc;
    Double weight;
    
    // to determine whether the ant is at home (origin)
    Bool is_at_home() {
    }
}
```

**Examples**

```
CHECK( (new Ant(new Posn(0,0), 0.0001))->is_at_home() == true );
CHECK( (new Ant(new Posn(5,10), 0.0001))->is_at_home() == false );
```

**Template**

```
Class Ant {
    Posn *loc;
    Double weight;
    
    // to determine whether the ant is at home (origin)
    Bool is_at_home() {
        … loc-> is_home() … weight ...
    }
}


class Posn {
    Int x;
    Int y;
    
    // To return the opposite post over the diagonal
    Posn *flip() {
       return new Posn(y, x);
    }


    //NEW - To represent origin
    Bool is_home() {
        … x … y …
    }
}


CHECK( (new Ant(new Posn(0,0), 0.0001))->is_at_home() == true );
CHECK( (new Ant(new Posn(5,10), 0.0001))->is_at_home() == false );
CHECK( (new Posn(0, 0))->is_home() == true );
CHECK( (new Posn(-3, 4))->is_home() == false );
```

**Body**

```
Class Ant {
    Posn *loc;
    Double weight;
    
    // to determine whether the ant is at home (origin)
    Bool is_at_home() {
        Return loc->is_home();
    }
}

class Posn {
    Int x;
    Int y;
    
    // To return the opposite post over the diagonal
    Posn *flip() {
       return new Posn(y, x);
    }

    //NEW - To represent origin
    Bool is_home() {
        Return (x == 0) && (y == 0);
    }
}
```

#### Example #4
**Representation**  
Track an animal, which is a tiger or a snake;  
Can’t make a class with multiple parts. We have two things , one OR this other one  
Data has variants, it needs a base class and subclasses. 

```
Class Animal {
    Virtual Bool is_heavy() = 0;
}

Class Tiger : public Animal {
    Std::string color;
    Int stripe_count;
    Bool is_heavy() {
        ...
    }
}

Class Snake : public Animal {
    Std::string color;
    Int weight;
    Std::string food;

    Bool is_heavy() {
        ...
    }
}
```

**Purpose**
Variants have the same method “is_heavy” but we don’t want to repeat its purpose. We put the purpose in the base class to say that it should do this specific thing for all animals.  
Variants can have specifics about what makes one “is_heavy"

```
Class Animal {
    // Move using more than one person
    Virtual Bool is_heavy() = 0;
}


Class Tiger : public Animal {
    Std::string color;
    Int stripe_count;

    Bool is_heavy() {
        ...
    }
}


Class Snake : public Animal {
    Std::string color;
    Int weight;
    Std::string food;

    Bool is_heavy() {
        ...
    }
}
```

**Example**

```
CHECK( (new Tiger(“orange”, 14))->is_heavy() == true );
CHECK( (new Snake(“green”, 10, “rats”))->is_heavy() == true );
CHECK( (new Snake(“yellow”, 10, “cake”))->is_heavy() == true );
```

**Template**

```
Class Animal {
    // Move using more than one person
    Virtual Bool is_heavy() = 0;
}

Class Tiger : public Animal {
    Std::string color;
    Int stripe_count;

    Bool is_heavy() {
        ... color ... stripe_count ...
    }
}

Class Snake : public Animal {
    Std::string color;
    Int weight;
    Std::string food;

    Bool is_heavy() {
        ... color ... weight ... food
    }
}
```

**Body**

```
Class Animal {
    // Move using more than one person
    Virtual Bool is_heavy() = 0;
}

Class Tiger : public Animal {
    Std::string color;
    Int stripe_count;

    Bool is_heavy() {
        Return true;
    }
}

Class Snake : public Animal {
    Std::string color;
    Int weight;
    Std::string food;

    Bool is_heavy() {
        Return weight >= 10;
    }
}
```

### Objects as Boxes
Say we have an aquarium, which has any number of fish, each with a weight. Multiple things to track.  
We don’t really know the final total components  
Class lets us define a new kind of box  
The box has many compartments as we want, but we have to pick that when we write the class  
Box compartments are stretchy to fit any one thing in each slot (string, int, float, etc)  
Even other boxes can go into a box slot  
The number of boxes is fixed a head of time  

Say you have 4 fish, and a bunch of two slot boxes. Each slot can have only thing. We only have one stamp to ship these fish.  
One fish in a slot results in 2 boxes overall. We can take those two boxes and put them in the two slots in another box

If there are 0 fish, use empty  
If you have a package and a new fish, put them together  
If you have many fish, start with empty on one side, and add fish one after another to the side.

```
(fish|empty)  
(Fish|(fish|empty))
(fish|(fish|(fish|empty)))
```

In code this is 

```
empty = new EmptyAq()
New BiggerAq(fish,aq);
Becomes 
empty
New BiggerAq( 10, empty )
New BiggerAq( 5, New BiggerAq( 10, empty ) )
New BiggerAq( 7, New BiggerAq( 5, New BiggerAq( 10, empty ) ) )
```

**Representation**
Track an aquarium which has any number of fish, each with a weight

```
Class Aq{
    Virtual Aq *feed_fish() = 0;
}

Class EmptyAq : public Aq {
    Virtual Aq *feed_fish() {
    }
}

Class BiggerAq : public Aq {
    Int fish;
    Aq *rest;
    Virtual Aq *feed_fish() {
    }
}
```

**Purpose**

```
Class Aq{
    // Return an aquarium after each fish is fed 1 pound of food
    Virtual Aq *feed_fish() = 0;
}

Class EmptyAq : public Aq {
    Virtual Aq *feed_fish() {
    }
}

Class BiggerAq : public Aq {
    Int fish;
    Aq *rest;
    Virtual Aq *feed_fish() {
    }
}
```

**Example**

```
CHECK( (new EmptyAq())->feed_fish()->equals(new EmptyAq()) );
CHECK( (new BiggerAq(2, new EmptyAq()))->feed_fish()->equals(new BiggerAq(3, new EmptyAq())) );
```

**Template**

```
Class Aq{
    // Return an aquarium after each fish is fed 1 pound of food
    Virtual Aq *feed_fish() = 0;
}

Class EmptyAq : public Aq {
    Virtual Aq *feed_fish() {
        ...
    }
}

Class BiggerAq : public Aq {
    Int fish;
    Aq *rest;
    Virtual Aq *feed_fish() {
        ... fish ... rest->feed_fish() … 
        // rest is an Aq. We need to call a method on it. Aq has feed_fish already so we don’t need to make a new one for it
    }
}
```

**Body**

```
Class Aq{
    // Return an aquarium after each fish is fed 1 pound of food
    Virtual Aq *feed_fish() = 0;
}

Class EmptyAq : public Aq {
    Virtual Aq *feed_fish() {
        Return new EmptyAq(); // can say return this;
    }
}

Class BiggerAq : public Aq {
    Int fish;
    Aq *rest;
    Virtual Aq *feed_fish() {
        Return new BiggerAq(fish+1, rest->feed_fish());
    }
}

CHECK( (new EmptyAq())->feed_fish()->equals(new EmptyAq()) );
CHECK( (new BiggerAq(2, new EmptyAq()))->feed_fish()->equals(new BiggerAq(3, new EmptyAq())) );
```