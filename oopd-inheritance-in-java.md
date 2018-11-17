#oopd inheritance in Java
## destillation of the Chapter 3.5 of the Java school book by Dietmar Abts
To use the inhertiance in Java simply add 'extends YourSuperClassName' to a Class. This will make this class a child of your Super class.

### Implications of inheritance
A child will inherit, all attributes and methods that are not set to private.
A child will not inherit the constructors!
A child can have its own attributes and overwrite super methods.

### @override aka overwrite a method
The annotation to overwrite a method in Java is to add @override on the first line.
Override is usefull when a subclass has to implement the method but doing it diffelently than in the Super class. The book gives the example of Accounts within a bank, both have the methods cashwithdraw. But the child overrides it and adds extra stepps to the method. Like the super class just pays out many regardless of balance, child has a set limit and only pays out if the limit is not reached.

### using same variable name in super and child
This will hide super variable under the child variable. But the super variable can be accessed by super.yourVariable.

### contructors
As mentioned constructors are not inherited but they can be reused in the child.
The super contructors can be easely accessed and used in the child constructor.
So only the child specific variables are set in the child, but the rest is set via the super contructor.

### root of inheritance -> Object
If the class does not inherit any class by default it will but hidden, then this inheritance comes from the mother of all, the Object class. Note Java is based on the single inheritance principle, this means only a sub class can extend just one super class, or one child can have only one parent.

### order of creation
If a programm creates a child object, the order of creation is:
1. super class object
2. child class object
Eventhough the super class object was asked for by the user, Java will follow this hirarchy for construction.

### upcast, downcast, asigned variables
Say Variable myVariable refers to a super class object. Lets link myVariable to a new sup class object. By calling myVariable.anyMethod() not the super method will be used but if it exists the sub method. Why because Java knows that the myVariable links to a sub class!
But how to acces a specific method form here that only the sub class has? Then you need the downcast.
'((SubClass)myVariable).mySubclassMethod();'
Then I can access a sub class method even using a Super class Variable.

### check for a class with instance of
Java allows to check for is this class part of that class. Instanceof will result in a boolean response of true or false.
Examples:
(myVariable instanceof MySuperClass) //Asks if the object referenced by myVariable is part of MySuperClass
(myVariable instance of MySubClass) //Asks if it is part of or rather is of this specific SubClass
