#oopd-superclass-how-to-reduce-redundant-information
This document not only shows how to reduce redundant information in classes but also describes how to desing classes from scratch.

## how to design classes
steps to desing classes, in order of the following chapters:
1. analyze the classes and its proberties in a table
2. create the classes in a class diagram
3. find super classes for object families and references/compositions @link oopd-reference-or-composition-of-classes.md

## how to indentify redundant information
given a simple grafic edior project which can draw squares, cirles and lines.

### analyze the grafic editor
for the three figures a table helps analyze their properties. thinking about the property of each fiugre one could come to the following description of needed properties for each figure.
``` md
Properties  	|Square				|Cirlce				|Line					|
------------	|----------			|--------			|-------				|
position x,y    |square top left	|circle center		|either end of line		|
width			|left to right		|
height			|top to bottom		|
radius			|					|center to cirlce	|
endpoint x,y	|					|					|other end of line		|
```

### create classes for each item
given the tables above, one can design the clases as uml class models, like so:
``` md
|Square				|Cirlce				|Line			|
|----------			|--------			|-------		|
|x:int				|x:int				|x:int			|
|y:int				|y:int				|y:int			|	
|width:int			|radius:int			|endX:int		|
|height:int			|					|enxY:int		|
```
to remove the x:int and y:int form all the classes the parent class can be formed:
``` md
|figure		|
|-----------|
|x:int		|
|y:int		|

^ [should be a closed white triangle in UML, to designate parent class]
|
-------------------------------------------------
|							|					|

|Square				|Cirlce				|Line			|
|----------			|--------			|-------		|
|width:int			|radius:int			|endX:int		|
|height:int			|					|enxY:int		|
```
To find the name of the parent class make a sentence like of the form:
Subclass is a Parentclass. Condor is a Bird. Human is a Mammal. etc.
