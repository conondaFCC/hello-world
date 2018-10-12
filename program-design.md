TODO - Once more principles are in this document grouping them would be nice.

# program design
first there are many approches to program design, the ones described here will most likely be for object oriented program design.
There may be several principles to become familiar with to make a well designed program.

## Onlin ressources
resource for design principles might be the page https://refactoring.guru/design-patterns also in general the SOLID principle is a good starting point https://en.wikipedia.org/wiki/SOLID or a nice explanation form STUPID to SOLID code https://williamdurand.fr/2013/07/30/from-stupid-to-solid-code/

## class based draft from an application description
this approach is taken from the Modules 226-1_09-12 from the TSBE.
This approach is described in the Module 226-1, with the submodules building up the skills to design a program from a given application description. So this approach starts with the intention of the program already described.
**Most likely the first draft might not represent the best possible solution, but this is ok. As long as you are willing to come back and change the draft and improve it as soon as a better way is found this is actually part of the learning process.**

### find possible classes in the description
One way to find possible classes in the description is to highlight all the nouns that are relevant for the application and then make a list of them.
Some of the nouns might not become a class but a class attribute instead. Some nouns are just not relevant for the program at all.

### choose 3-4 nouns from the list
Next step choose 3-4 nouns from the list where the chance of them being a class is very high. A help might be, think about what objects might be used in the program at runtime.
For those 3-4 descibe the responsibilty of those classes in no more than 3 sentences. If you need more than 3, most likely the class has more responsibilty than one and fails to meet the SRP (single-responsibilty-principle).

### find the class attributes
Think what information do the objects need for their responsibilty? If this is not clear check the list from above many nouns should be attributes and not classes. If a sentence like 'noun1 has a noun2' is possible it is likely that noun2 is an attribute.
This task should be done on paper or Modelio and result in a firt draft of a class diagram.

### find the class methods
Look in the description for verbs bounded to class nouns. Underline those verbs and add them to the class diagram. Think about each verb as a method and if it feels like one think about its parameters and describe them aswell.

### find the relations between the classes
A relations is a reference to another object. Often these are instance variables with the data type of a class. If not abvious the possible sentence might help: 'an object knows one or more objects'
Look for those relations in the description and define the multiplicity, navigation, and role name for all of them and add them to the class diagram.

### create the java code of the draft
If done in modelio this is straight forward see modelio.md for more info. Otherwise create the classes with all else in our favourite programming enviroment.

## delegation principle
this one kicks in after the first UML class draft has been made and code is beeing written. What it means? Not everything has to be done in one class aka one object. Instead let other objects do stuff with methods with their own objects and then alter the target values of the starting object.

Another way to wrap up delegation:
- objects call methods on other objects at runtime, which do certain tasks for the caller object.

### How is it done?
One example is you have some code and one class does many things. This is against the single-responsibilty-principle aka the SRP. Beginners often put code into one single class. Using delegtion would be to outsource bits of code into new classes. For example input and output related code into a class called UserInterface. There the input would be catched and saved. Another class could check the input. Lets call it Validator class. This class separetes the input into single characters for example and checks that every charater corresponds to the input pattern expected. Furthermore the user input will be translated into commands in the Command class. Whereas the UserInterface creates a Validtor object which returns a Command object. 'UI.readinput() reads input, creates new Validator object, calls Validator.checkInput method if true, return validator.createCommando'
Obiouvsly this approach needs a lot of experience to become good at it. The only way to learn it is to refine made design over and over again applying the SRP.
