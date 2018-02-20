# UML class diagram
## Source for this documentation
- https://en.wikipedia.org/wiki/Class_diagram and in german https://de.wikipedia.org/wiki/Klassendiagramm (Since the german page is more detailed on certain aspects)
## Other sources
- maybe the best book and teachers from Vienna: http://www.uml.ac.at/en/, good slides for Use-case-diagram do's and don'ts, and class diagram associations and others
- The mother of UML: http://www.uml.org/, good general description and link to Specification

## Content
- This documentation tries to explain UML class diagram in own wording
- How to read and draw class diagram
- How to draw class diagram automatically with existing Java code
- How to use modelio

## Introduction to class diagrams
Why use class diagrams?
Class diagrams are a way to represent a program and what it does in different detailed scales. So to understand what a program does and how class diagrams are very helpful.
There are other diagrams defined by UML (Unified Modeling Language) to represent a program and what is does.

"The **class diagram** is the **main building block of object-oriented modelling**. It is used for general conceptual modelling of the systematic of the application, and for detailed modelling translating the models into programming code. Class diagrams can also be used for data modeling. The classes in a class diagram represent both the main elements, interactions in the application, and the classes to be programmed." from wikipedia

## Class representation
How to show a class in an UML class diagram?

| Class          |
| :------------- |
| attributes     |      
| operations     |

One box represents the class, with three sub segments:
 - **ClassName** always Bold, 1st segment
 - **attributes** (class & other variables), 2nd segment
 - **operations** (Constructors & Methods), 3rd segment


More Detailed class example:

| **ExampleClass**          | description                         |
| :------------------------ | :---------------------------------- |
| - exampleVariable:integer | [visibility] attributes name : type |
| + ExampleMethod(in exampleParameter:ExampleClass, in exampleParameter:integer)| [visibility] Method ([paramterHandleInfo] ParameterName:type, ...)

Note if more than one attribute or operation exists then they are all written in the according box segment.

#### Specials for Attributes
##### / prefix
If an attribute starts with an / it means this attribute is made of another one. Example:
dob:date
/age:int
Date of birth must be known, age will be made out of dob.

##### values entered with =
If an attribute has an = sign on the line this represents the standard value. Example:
password:String = "pw1234"

##### instance variables and class variables
- instance variables are the normal variables that can be defined on instance base. Ex. speed
- class variables are class attribute or static attribute and as such defined only once per class for all instances. Ex. counter of how many instances made, constants, etc.

##### underlined attributes
to be explained (equivalent of static or final in) Java)

### Visibility legend

| sign | visibility |
| :--- | :--------- |
| +    | public     |
| -    | private    |
| ~    | package    |
| #    | protected  |

### Prefix for Parameters
If a Method is fed with parameters, in UML class diagram these parameters can be specified as follows:

| short | Specification         |
| :---- | :-------------------- |
| in    | input (read only!)    |
| out   | output (write only!)  |
| inout | read, do, write       |

### Difference between Constructor and Method representation?
If the type is missing it is a Constructor. Also Contructors start with capital, Methods not.
Ex.:
- ClassName () = Constructor
- ClassName(Paramter:type) = Constructor
- notClassName():**type** = Method
- notClassName(Parameter:type):**type** = Method

### Static attribute or operation
Static attribute or operation are underlined.

## Relationships
Classes can have relationships. Relationships are represented by **lines connecting two classes**. Each relationship is explained in detail in its own sub chapter.

List of possible relations:
- Association
- Aggregation
- Composition
- Inheritance
- Dependency

| Representation            | Relation    |
| :------------------------ | :---------- |
| --- (single line)         | Association |
| white diamond at one end  | Aggregation |
| black diamond at one end  | Composition |
| tbd                       | Inheritance |
| --- (dotted line)         | Dependency  |

When do two classes have a relationship?
The use of a class in other class. Example: Class A has a variable 'myBObj' of the class data type 'B'. Therefore Class A has a relationship with class B. See chapter Multiplicity for Javacode Examples.

### Intermezzo: class connecting lines
If a relationship line has text centered and above it, this is the description of the relataionship. On each end of the line ample information can be added for:
- Visibility and Role (above the line)
- Navigation (signs on the line)
- indicator of Multiplicity (underneath the line)

UML example:
1st line shows the legend, 2nd line shows one end of a connecting line
with a class (line should be unperforated!):

```
legend:
   [Visibility] [Role]
----------------------- [arrow, x or blank line = Navigation]
        [Multiplicity]

end of line connected to class:
        - referenceToB   -------------------
-----------------------> | B (=ClassName)  |
                    1    -------------------
```
#### Visibility
Indicates Visibility signs are already described under Visibility legend.

#### Navigation
Ex. of Navigation options

| Sign | Navigation             |
| :--  | :--------------------- |
| ---  | undefined              |
| -->  | target has a reference (instance variable) that allows access |
| <->  | both classes have a reference to each other  |
| -x-  | no navigation possible |

#### Multiplicity
Shows the number of objects that the class "A" has relationships with class "B" at runtime.

Some ex. of Multiplicity and what they mean:

``` JAVA
// 0..1 Multiplicity
class A {
  B myBObj; // reference to B-object
}

class B {}

// * Multiplicity (zero or more instances)
class A {
  B[] myBObjs; // reference to B-array
}

class B {}

// * Multiplicity (zero or more instances)
class A {
  Vector<B> myBObjs; // vector with B-Obj
}

class B {}

// 1 <---> * (two way navigation)
class A {
  Vector<B> myBObjs;
}

class B {
  A myAObj;
}
```

### Association
Example of an Association between Try and Code class, the line would a solid line!

```
-------                   - secretCode   ---------    
| Try |   -----------------------------> |  Code |
-------                             1    ---------  
```

### Aggregation
Aggregation is a variant of Association. It tells next to the Association that one class is part of the other class like a Professor has a Class to teach or a Pond has Duck's in it. The Pond and the Duck can exist for itself but together they form something more then the parts alone.

The symbol for Aggregation is a hollow diamond.

```
---------                               ---------    
| Pond  |  /\ ------------------------> |  Duck |
---------  \/  0-1                 0-*  ---------  
```

### Composition
The Composition is a stronger form of an Aggregation. The existence of one part is dependent on the other part. Composition may be used representing real world whole-part relationships (example engine is part of a car) or when the container gets destroyed the contents get also destroyed (example university and its departments).

The symbol for Composition is a black diamond.

```
---------------                     --------------    
| University  |  /\ --------------> | Department |
---------------  \/  1         1-*  --------------  
```
Note Composition on the whole-part side always one not bigger than one!

### Inheritance
tbd not yet..

### Dependency
Dependency says that one class is needing the other class in on way. (Can it be pinned down to that using an other classes public method results in the << use >> dependency?)

```
-------                      ----------------------   
| Car |                      | Engine             |
-------                      ----------------------   
|     |   ................>  | + accelerate():int |
-------      << use >>       ----------------------
```

The line for dependency is dotted!

In this case car uses accelerate. If accelerate is changed car has to be tested also.

Many other dependencys can exist. The main identifier is the word in the << tag >> between the brackets.
Often only the most important dependencys get depicted in the diagram for readability.

## create UML class diagram with JAVA code via eclipse
Follow this short video: https://www.youtube.com/watch?v=0Zlh56mTS6c
Steps are:
- install http://www.objectaid.com/installation
- in Eclipse install just the part "license add-on"
- in Project view add new with Ctrl+N, add objectaid Class Diagram
- name it (this should add an .ucls file in the top of the folder hirachy)
- drag class.java files into the .ucls file
- DONE

## Modelio
### Online Guide to modelio
http://scribetools.readthedocs.io/en/latest/modelio/index.html
Describes what you can do with Modelio and what you should not! Most noteworthy:
* Modelio works fine as a single user application, dont share it with googledrive or git, it wont work or give problems
* to back up use the archive feature (creates .zip) to reload use imprt on zip file

### Three Levels in software design with Modelio
* analysis phase, define requirements,  realize  use  cases,  construct design  models  and  use  all  of  UML’s  modeling capacities. Model construction and documentation generation are results of this phase.
* design phase focuses on realization, building productive models that concentrate on the main  architectural themes and essential system classes.
* The  coding/detailed  design  phase  completes  the  general  design  model  in  roundtrip mode,  by using the features of the Eclipse/Java environment and by permanently synchronizing the model
copied from: https://www.modelio.org/download/send/5-white-papers/1-improve-your-java-development-efficiency-en.html
