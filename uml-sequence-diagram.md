# UML sequence diagram
## Source for this documentation
- https://en.wikipedia.org/wiki/Sequence_diagram
- http://my.safaribooksonline.com/book/software-engineering-and-development/uml/0321193687/sequence-diagrams/ch04 by Martin Fowler, is a external link on main wiki article, this link has subchapters that are also relevant for create, delete participants, loops, conditions, When to Use Sequence Diagrams, CRC cards [class-relation-collaboration card method] etc.
- good basics explanation on why and how to use the sequence daigram from IBM, https://www.ibm.com/developerworks/rational/library/3101.html

## Other sources
- Theory Script from Module 226A-11
- worthy mention: http://www.tracemodeler.com/articles/a_quick_introduction_to_uml_sequence_diagrams/ is a external link on main wiki article

## Notable quotes
"A common mistake is to try to create a complete set of sequence diagrams for your system. I've seen project teams waste months creating several sequence diagrams for each of their use cases, one for the basic course of action and one for each alternate course. My advice is to **only create a sequence diagram when you have complex logic that you want to think through – if the logic is straightforward the sequence diagram won't add any value, you had might as well go straight to code.**" from Scott W. Abmler, http://www.agilemodeling.com/artifacts/sequenceDiagram.htm

## Content
- Explain UML sequence diagram in own wording
- Give hints and links to read, enable to understand and draw a sequence diagram
- How to draw sequence diagram in Modelio with existing Java code
- How to draw sequence diagram **automatically** with existing Java code via architexa? there seems to be no complete automation for this.. http://www.architexa.com/support/videos/sequence-diagrams/video

## Introduction to sequence diagrams
### what to represent in the sequence diagram?
It could show the flow of a method or it could show the flow of the method with a given input. The first would explain the method in general, the 2nd would show a specific use case.

### naming sequence diagrams
syntax: DiagramType DiagramName : className, exmaple: sd Balance Lookup (int accountNumber) : int

## Draw sequence diagram in Modelio
Prerequisite, existing code, existing Modelio project, updated project inside Modelio.

### create new diagram
In Modelio, create a new diagram inside a folder/package via right click > create diagram > sequence diagram.
Chose what case the diagram will represent. Drag all classes into the new diagram space that should be represented whit a life line. (For anthor diagram using the same objects or instances, drag the instance from the 'locals' folder into the new diagram!)
Typically start with the one instance of an object that is created in the main method. Then add the class which method it will use.
Insert a 'synchronus message' from the Message Tools between the two classes for a method with a return value. Insert a 'Asynchronus Message' when method is without return value (unconfirmed sync & async message useage). This will represent what method is called. Select the actual method in the menu under properties > invoked. This will list all available methods from this class.
Then follow the method and represent what is does.

### Draw loops, conditions etc.
Insert a combined fragment from the Nodes section into the sequence diagram. Often it is adviced to expand the lifeline first of the method by draging the center of the return arrow down.
The combined fragment has an operator that represents what it stands for loop, condition etc.

: Combinded fragment operator 	: meaning :
:-------------------------------:---------:
: loop							: loop :
: Opt							: if :
: Alt 							: if, else :

loop example, do while:
the loop has a guard element. This represents the trigger/condition for the loop.

creating an object and representing it in the sequence diagram, within a method
create the lifeline for the class by draging it into the diagram. Select from the Messages group, the 'Creation message' then draw the arrow from the current method to the new class. This will take down the class creation and lifeline to the current action.
To represent the construction of the object create another Message, choose synchronus message, connect the two lifelines, then select under property > invoked > the used constructor.
To show the argument variable used with the constructor, add it in the corresponding field.
If the constructor is shown in the diagram, the question will be to show or not the steps inside the constructor.

To represent 'String eingabe = scanner.nextLine();' apart from the method use in the 'synchronus message' the Variable can shown in the diagram by clicking the response arrow of the message, and adding as Name: eingabe = nextLine(). The syntax is 'variableName = usedMethod()'.

To represent a call to method within the class use the 'internal message' type.

To represent if, else if, else, in one combined fragment use the 'Interaction operand' to separate each one and add another element with a guard. For the last else put 'else' as guard term.

#### if else example with return
To represent an if else condition with a return in the if condition equals a break statement. So do not use the combined fragment as an optional instead use it with break as the operator.

Example JAVA code:
´´´ Java
if (validator.istGueltig())
	return validator.erzeugeKommando();
else
	zeigeFehlermeldung();
```

How to represent just the 'return validator.erzeugeKommando();'?
Easy make a synchronus message from caller to Validator, the invoked method is 'erzeugeKommando()' the return value is the same! Wrap it inside a Break combined fragment. Outside of it add an inner synchronus message with the invoked method 'zeigeFehlermeldung()'. This is just one simple solution. For more Detail one could create a new lifeline for the new object Kommando etc.

#### Variable assignment example
How to represent the code 'istGueltig = false'?
AFAIK this is not meant to be represented in the sequence diagram, it might be in the action diagram which covers more details, but here this would be to detailed information. But if you really want to show it, one way is via Notes/Description fields.

#### try catch example
Again like the assigning a variable to a value it is not defined how to represent a try catch scenario in a sequence diagram. Refer to the next link and the break explanaition from the IBM link alos. https://medium.com/zenuml/how-i-draw-a-try-catch-in-a-sequence-diagram-9eed8f805589
The link above basically says anything goes as long as the reader gets the intention of what you try to say.

#### java switch case example
https://www.oreilly.com/library/view/javatm-how-to/9780133813036/ch05lev2sec23.html
and maybe still more crucial the IBM article mentioned in the source for this notes on the top!

#### reference another sequence diagram example
Again according to IBM link (see above):
Naming the reference, syntax is: sequence diagram name[(arguments)] [: return value] -> Example: 'sd cell uncover(line, row)'.
In modelio the reference box aka UML therm interaction occurence, Modelio therm: 'Interaction Use' under Nodes, should be used to draw the box.

In case the return value of the method is an object, the last object, the return value, instance is named like the sd name. See fig. 12 in IBM link.

** WARNING** In Modelio use a new Interaction for each diagram or sequence diagram since otherwise deleting nodes will delete it on the other diagrams too. As long this beheaviour is not understood better use a new Interaction aka make a new sequence diagram on the top folder or packet folder.
