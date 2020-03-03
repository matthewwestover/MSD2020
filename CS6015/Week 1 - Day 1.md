## Week 1 - Day 1
A few simple ideas can vastly improve software quality  
“Things to know how to be better at programming”  
Core concepts for being a good software engineer  
Software requirements and specifications  
Testing - test coverage, continuous integration, randomized testing  
Assertions  
Code structure  
Documentation  
Static and dynamic analysis  

Building **language interpreters** is a rich source of:  
Example programs for good engineering practice  
Insight to programming generally 

As a programmer what can you do to be more effective?

**Arithmetic Exercise**
Allow different numbers to do basic +,-,*,/  
Can be 2+ numbers present  
Parenthesis matter  
This is difficult to **parse**

We can use **Grammar** as a specification for what we want this program to do

### Grammar

```
<expr> = <number>
<number = <digits>+ (at least one digit)
<digit> = [0],[1],[2],…[8],[9]
| ( <expr> )
| <expr> + <expr>
| <expr> * <expr>
```

This does not specify the order of operation
1+2\*3 can be seen as 1+2 * 3 or 1 + 2*3

### Refined Grammer
```
<expr> = <addend>
| <addend + <addend>

<addend> = <number>
| ( <expr> )
| <addend> * <addend>

<expr> or <addend> always starts with <number> or (
+ after an <addend> implies <expr>

```
