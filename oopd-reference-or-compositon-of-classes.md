#oopd-reference-or-compositon-of-classes
## reference or composition with inheritance
one mistake many beginners make often starting with object oriented program design and the involved inheritance, is not using the reference class or composition class, when needed.

the lesson gave me the example of classes car and lorry, both having the engine/powerplant option of gas or electric, instead of allowing each car, lorry to have the option for the engine/powerplant it is best to reference the engine/powerplant to a reference class called engine/powerplant which consists of two classes gas and electric.

## how to spot the need for a reference class
it is said that this needs experiance. one way I can sum it up is if an object family is made of another object family, then it might be good to make a reference class.

## examples
Some examples of object families made of other object families:
Automobiles -> Engines, Cloth -> Fabrics, Furniture -> Materials