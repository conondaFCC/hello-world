# JAVA tips

## Do not create existing Classes
Do not create a class that is already existing in Java Source code (JDK | JRE). Example if you create a class List, this will "break" the existing class List from native Java and unexpected things might happen!

## make large numbers easy to read
In Java long numbers can be made more readable with underscores. Exmaple:
``` JAVA
int longNumber = 7_000_000; // the underscores will be ingored by the Compiler
int binaryLongNumber = 0b1010_1001_0001;
```
## Operators, how not to use them
Imagine the + operator used in variable. It does not work like this!

```JAVA
char operator = '+';
System.out.println(1 + 2); // = 3
System.out.println(1 + operator + 2); // = 46, why 46?
System.out.println(0 + '+'); // = 43, '+' = 43!!!
```
The char '+' gets converted to an int number, it does not keep its function as an + operator!!!

## Code Style aka good programming
Style refers to how to write good code.
- comment each variable for readability and information
``` JAVA
double principal;    // Amount of money invested.
double interestRate; // Rate as a decimal, not percentage.
```

## Subroutines
What is a subroutine? Every bunch of code that can be called by a name and then will be executed is a subroutine. In Java every method is subroutine.
``` JAVA
// calling the class System and any subroutine
System.exit(0);
// or class Math
Math.PI;
Math.E;
```

## Code snippets
### Math
Watch out some Math subroutines have a given return type others adjust to the input data type!

### The == vs equal problem
The == operator for reference types tests for reference *equality* (i.e. the *same object*). Therefore, in the first example int1_1 == int2_1 is true because the references are the same. In the second example int2_1 == int2_2 is false because the *references* are different.
From common pitfalls in JAVA: https://stackoverflow.com/documentation/java/4388/common-java-pitfalls#t=201707191752057946584

### System.out.printf
The function System.out.printf can be used to produce formatted output. printf = print formatted
``` JAVA
int x = 1, y = 2;
System.out.printf("The product of %d and %d is %d", x, y, x*y);
// output: The product of 1 and 2 is 2
```


## Arrays
What is an array?
An Array is a defined List of memory space for a defined data type and a defined number of values. Think of it like an excel table where you know what data type you will insert and how many rows you need.

It is possible to make a multi dimensional Array.

### Array vs ArrayList
| Array 						| ArrayList     								|
| :----------------------------	| :-------------------------------------------- |
| limited items, fixed length	| unlimited items, auto expands					|
| over limit = error			| over limit = auto expands						|
| one data type					| all kind of data or one defined class as type |
| data type primitive or class	| data type String or defined class or classes*	|
| faster						| slower										|
| initialized with base values	| can be initialized without content (empty) 	|

\* primitive types can be converted to objects via wrapper classes
(Double, Integer, etc.), each primitive type has its own wrapper class.
Remember primitive types are not classes or objects! For more info on this read
about wrapper classes and autoboxing, where the conversion from primitive type
into an object/class is done automatically!!

``` JAVA
int[] intArray = new int[10]; // init Array with 10 fields
System.out.println(intArray.length); // Output: 10

ArrayList myArrayList = new ArrayList(); // init ArrayList with 10 empty fields
System.out.println(myArrayList.size()); // Output: 0!!!
ArrayList myArrayList2 = new ArrayList(5); // init ArrayList with 5 empty fields
System.out.println(myArrayList2.size()); // Output: 0!!!

```

### Vector vs ArrayList
Vector is synchronised, ArrayList is not. (Not clear yet what this means...)

### Normal Arrays
Normal Arrays can be created in two ways and contain two types of data either
primitives or objects. Example:

``` JAVA
// array-type[] array-variable = new base-type[array-length];
int[] firstArray = int[10];  //creates array for storing 10 int values
int[] secondArray = {1,2,3,99}; // creates array with 4 values stored (1,2,3 and 99)

int arraySize = 5; // variable for arraySize
String[] thirdArray = String[arraySize]; // creates array with place to store 5 Strings
```

To be clear declaring an array does not make the array!
Only initalizing the array with **new** creates it!!! Example:

``` JAVA
int[] firstArray; // declearing an Array
firstArray = new int[10]; // initalizing the Array
```

#### Initial values of Arrays
| type 		| initial value					|
| :-------- | :---------------------------- |
| int 	    | zero       					|
| boolean	|	false						|
| char		|	Unicode char with number zero*|
| String	|	null						|
| object	|	null**						|
\* Standard values of a char[3] Array: [ , , ]
\** To acces the objects they need be initialized! See code below:

''' Java
class ObjA {
	int a = 1;
}

public class ArrayObjects {

	public static void main(String[] args) {
		ObjA[] objAArray = new ObjA[3];
		System.out.println(Arrays.toString(objAArray)); // [null, null, null]

		objAArray[0] = new ObjA();
		System.out.println(objAArray[0].a); // 1
		
		for (int i = 0; i < objAArray.length; i++) {
			objAArray[i] = new ObjA();	}
		System.out.println(Arrays.toString(objAArray)); // [test.ObjA@515f550a, test.ObjA@626b2d4a, test.ObjA@5e91993f]
	}
}
'''

### Array.length, how to use and what to look out for

Be aware of the following! Using the array.length in a loop is not the same as using it as an index!
``` Java
//example loop, loop goes till the end of the array like intended
String zeile = "";
for (int i = 0; i < spielfeld.zellenArray.length; i++) {
	zeile = zeile+"  "+i+"  ";
	for (int j = 0; j < spielfeld.zellenArray[i].length; j++) {
		zeile += spielfeld.zellenArray[i][j].zeichen+"  ";
	}
	zeile += "\n";
}
return zeile;

//examples reference as index, goes out of bound!!
// but refering to the length to reference to to position in array via the .length method might not give the hoped result!
spielfeld.zellenArray[0][spielfeld.zellenArray[0].length].setzeBombe(); //length is +1 than reference to last cell! 
//therefore to reference to the last cell it should be .length-1!

// but this works again without problems!
for (int i = 0; i < gewuenschteBomben; i++) {
	int randomInt = (int) (Math.random()*anzahlZellen);
	int zeile = randomInt/spielfeld.zeilen;
	int spalte = randomInt%spielfeld.zeilen;
	if (spielfeld.zellenArray[zeile][spalte].bombe == false) {
		spielfeld.zellenArray[zeile][spalte].setzeBombe();
//				System.out.println("Bombe auf zeile, spalte:" + zeile + ", " + spalte); //Kommentar ausblenden für detailierte Ausgabe
	}
```

### Create a multi dimensional Arrays

``` JAVA
// int[ ][ ] two-Dim-Array = new int[valueFirstDim][valueSecondDim];
int[][] twoDimArray = new int[10][100];
```
Example for two Dimensional Array? Imagine collecting data samples (temperatures for ex.) every six hours in three different locations. To represent one day you will need a two dimensional array.

To represent several days, a three dimensional array is needed. For seven days it would look like this:

``` JAVA
// int[ ][ ][ ] threeDimArray = new int[days][locations][samples];
int days = 7;
...
int[][][] threeDimArray = new int[days][locations][samples];

```

### more on multi dimensional Arrays
How to count the fields in a multi dimensional Array?
``` Java
Cell[][] cellArray = new Cells[2][3];
System.out.println(cellArray.length); // prints only first dimension

int cells = 0;

for (int i = 0; i < cellArray.length; i++) { //this loop runs through all cells
		for (int j = 0; j < cellArray[i].length; j++) {
			cells += 1;
		}
	}
System.out.println(cells); // result = 2*3 = 6
```

### How to Print the whole Array Content at once (Array, List (ArrayList, etc.)

``` JAVA
System.out.println(Arrays.toString(arrayname));
// Output: [Array[0]content, Array[1]content,...]

// for List objects print variable or use .toString = same result
System.out.println(arrayListName);
System.out.println(arrayListName.toString());
// Output for both: [ArrayListName[0]content, ArrayListName[1]content,...]
```

### how to copy an Array
1. for loop
2. clone
3. System.arraycopy(); // system method

## List (child of Collections)
Lists in JAVA and what you can do with it.

A very good Introduction to List is: http://tutorials.jenkov.com/java-collections/list.html

### Hint for import
make sure to import java.util.list and not java.awt.list when using List!

### List general usage
Store values. List vriants ArrayList, LinkedList, Stack, ...

ArrayList by default stores mixed type Data (int, String, object at the same time!) or generic data with a prefix see code example.

Example ArrayList:
``` JAVA
public class ListExamples {

	public static void main(String[] args) {
		// 		@SuppressWarnings("rawtypes")
		//		List myList = new ArrayList(); // was before auto cleanup with ctrl+1
		List<Comparable> myList = new ArrayList<Comparable>(); // exchange Comparable with a class for ex.

		String b = "this is b as variable";

		myList.add("Object 1");
		myList.add(123); //error with access via Iterator! bad idea to mix types
		myList.add(b);
		myList.add(b + " 666");

		//direct access
		int secondPosMyList = (int) myList.get(1); // 1 refers to position like with array
		System.out.println(secondPosMyList);

		//remove object or index
		myList.remove("Object 1");
		//myList.remove(123); // does not work cause 123 would refert to positon not number
		myList.remove(0); // 0 refers to position like with array

		//access via Iterator
		@SuppressWarnings("rawtypes")
		Iterator<Comparable> iterator = myList.iterator();
		while(iterator.hasNext()) {
			String element = (String) iterator.next();
			System.out.println(element);
		}

		//access via new for-loop = for(int i = 0; i < myList.size(); i++)
		for(Object object : myList) {
			String element = (String) object;
			System.out.println(element);
		}

		//List size
		int listSize = myList.size();
		System.out.println("List Size = " + listSize);

		//clear List entries
		myList.clear();
		System.out.println("List Size = " + listSize); //interesting pointer to old value?
		System.out.println("List Size = " + myList.size()); //actual value

		System.out.println(secondPosMyList); //still works cause pointer goes to value 123
		//		System.out.println(myList.get(1)); // creates exception IndexOutOfBoundsException:

		//generic ListArray = ListArray with type bounding
		List<String> myListStr = new ArrayList<>();
		myListStr.add("def");
		myListStr.add("ABC");
		//		myListStr.add(123); // now throws error: Unresolved compilation problem
		myListStr.set(1, "bcd"); // overwrites entry [1]

		@SuppressWarnings("unused")
		String position0 = myListStr.get(0); //this still is not smart, after sorting of myListStr
		String position1 = myListStr.get(1); //positions will not be updated !!!

		//print whole ArrayList:
		System.out.println("Whole ArrayList as syso: " + myListStr.toString());
		System.out.println(position1);

		// ListArray bound to Class or object
		Spielfeld sf = new Spielfeld();
		sf.initialisiereZellenInArray();
		
		// Try to store object through attribute of object, this does not work!
		List<String> zellen = new ArrayList<String>();
		zellen.add(Spielfeld.zellenArray[0][0].zeichen);
		System.out.println("zellenArray: "+zellen.toString());
		for (
				@SuppressWarnings("unused")
				String string : zellen) {
			string = "%";
		}
		System.out.println("zellenArray: "+zellen.toString());

		// store object in list, then modify static attribute
		List<Zelle> listCellObjects = new ArrayList<Zelle>();
		listCellObjects.add(Spielfeld.zellenArray[0][1]);
		System.out.println("listCellObjects: "+listCellObjects.toString());
		for (Zelle localVaribleZelle : listCellObjects) {
			localVaribleZelle.zeichen = "&";
		}
		System.out.println("listCellObjects: "+listCellObjects.toString());
		System.out.println(listCellObjects.get(0).zeichen);
		System.out.println(Spielfeld.zellenArray[0][1].zeichen);

		//sort List
		myListStr.sort(null); //.sort(null) stands for natural order
		System.out.println("Whole ArrayList as syso: " + myListStr.toString());
		System.out.println(position1); // is not position1 after sort!!
		myListStr.set(1, "a new entry");
		System.out.println(position1); // is not position1 after set [1] !!!!
		System.out.println(myListStr.get(1)); // how to get entry [1]

	}

}
```

## Random Number
Or how does creating a random number work?

#### The Easy Way (Math.random())
``` JAVA
double myrandom = Math.random();
System.out.println(myrandom); // prints doubles in the range of < 1.0 and > 0.0

int randomInt = (int) (Math.random()*99+1) // = numbers 1-99 as random
```
Input         | Output
--------------|-------
myrandom      |0.012.., 0.85.., 0.63.., 0.99.., etc.
myrandom*10   |0.12.., 8.5.., 6.3.., 9.9.., etc.
myrandom*10+1 |1.12.., 9.5.., 7.3.., 10.9.., etc.
downcast myrandom*10+1 <br> with (int) myrandom  | 1, 9, 7, 10
myrandom*20   |0.24.., 17.xx, 12.6.., 19.8.., etc.
myrandom*20+1 |1.23.., 18.xx, 13.6.., 20.8.., etc.
myrandom*10+20|20.12.., 28.5.., 26.3.., 29.9.., etc.

## Exception Handling - try and catch and throw
What do I know about this topic?
* How to read the stack trace aka exeption/error message? This means how to debug an error: resumee and link in sub chapter below
* Many Standard Exceptions exist, also it is possible to write your own Exceptions or rewrite Output and Triggers for a existing Exception. Some more detail see Grundkurs Java from p. 99 or https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html, or  https://stackify.com/specify-handle-exceptions-java/

#### how to read an exeption/error message? How to find the problem causing code
quick resumee of https://stackoverflow.com/a/3988794/7698264
* note that the root cause is the last cause mentioned! so work the error message form bottom to top!
* check for your code when other libraries are used, chances are your code is wrong not the lib.
* the main cause for the error should then be the first line refering to your project, your own code, form the bottom up! click it to jump there.
* for example code see link above

#### What Is an Exception?
An Exception is an Event that disrupts the intended flow of the program. https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html

This can be user Input not as expected, I/O error or others.

#### Why Exceptions exist?
It separates code that handles the flows of the program from code that handles only the errors, if this doesn't make sense see this good example: https://docs.oracle.com/javase/tutorial/essential/exceptions/advantages.html

#### Exception types - The Three Kinds of Exceptions
* checked Exceptions (for example user input not as whished)
* unchecked exceptions (Error and runtime)
##### Examples:
- checked exception, false user input, try and catch, and throws done by programmer. A good program anticipates and recovers from those
- unchecked exceptions, I/O error, cant be handled by program (might be hardware propblem, try catch not possible but throwing?) should give feedback where problem occured
- unchecked exceptions at runtime, usually a bug, fix it when found
* Here's the bottom line guideline: If a client can reasonably be expected to recover from an exception, make it a checked exception. If a client cannot do anything to recover from the exception, make it an unchecked exception.
#### Where is the exception cause defined aka the trigger?
First, understand there are premade Exceptions for common cases. This means the existing Exceptions should be used. How to know which Exception to use? Good question. Best answer at the moment, know which one you need. See example code for Exception description.

Second, the trigger aka the catch is somewhat tricky to see and explain because sometimes as understood it is not needed to be written and sometimes it is.

Throw or Trigger example self made code, see that the triggers are defined by the do>try>if statements:
``` Java
 /**
  * Check User Input via Exception
  */
 private static void checkUserInputForErrors() {
   do {
     try {
       //XXX 1011 and 6667 gets catched, but 1667 not, this would need
       // a new check system which checks each digit not the whole number!

       if (6666 < userInput) {
         throw new Exception("Guess is over 6666! Guess needs to be under 6667.");
       }
       if (0 < userInput && userInput < 1110) {
         throw new Exception("Guess is under 1111! Guess needs to be over 1110.");
       }
     } catch (InputMismatchException e) {
       System.out.println("Guess invalid!");
       userInput = -1; // restart userInput() by setting userInput below zero;
     } catch (Exception e) {/*XXX why use the class Exception and not the expected exceptions?
       *This is not good code! Worse would be the general Exception first, then InputMiss..
       *would never be considered */
       System.out.println("Guess: " + userInput + ", is invalid. "
           + "needs to be between 1111-6666.");
       userInput = -1; // restart userInput() by setting userInput below zero;
     }
   } while (userInput < 0);
 }
```

Multicatch example:
´´´ Java
do {
try {
} catch (exception1 | exception2 exceptionVariable) { do action if exceptions triggered}
´´´
Note for multicatch: all exceptions have to be of the same class hirarchy, mixing sub and parents is not allowed.

Two exception source examples:

``` JAVA
package java.util;

/*
 * Thrown by a <code>Scanner</code> to indicate that the token
 * retrieved does not match the pattern for the expected type, or
 * that the token is out of range for the expected type.
 *
 * @author  unascribed
 * @see     java.util.Scanner
 * @since   1.5
 */
public
class InputMismatchException extends NoSuchElementException {}
```
or
specially check for the method at the end which lets the **user add the throwable cause**. This cause just means the user can give an own description (name) for the cause (and the corresponding message to it). The trigger has to be made as shown in the trigger example.

``` JAVA
/*
 * Copyright (c) 1994, 2012, Oracle and/or its affiliates. All rights reserved.
 * ORACLE PROPRIETARY/CONFIDENTIAL. Use is subject to license terms.
 *
 */

package java.lang;

/**
 * Thrown to indicate that a method has been passed an illegal or
 * inappropriate argument.
 *
 * @author  unascribed
 * @since   JDK1.0
 */
public
class IllegalArgumentException extends RuntimeException {
    /**
     * Constructs an <code>IllegalArgumentException</code> with no
     * detail message.
     */
    public IllegalArgumentException() {
        super();
    }

    /**
     * Constructs an <code>IllegalArgumentException</code> with the
     * specified detail message.
     *
     * @param   s   the detail message.
     */
    public IllegalArgumentException(String s) {
        super(s);
    }

    /**
     * Constructs a new exception with the specified detail message and
     * cause.
     *
     * <p>Note that the detail message associated with <code>cause</code> is
     * <i>not</i> automatically incorporated in this exception's detail
     * message.
     *
     * @param  message the detail message (which is saved for later retrieval
     *         by the {@link Throwable#getMessage()} method).
     * @param  cause the cause (which is saved for later retrieval by the
     *         {@link Throwable#getCause()} method).  (A <tt>null</tt> value
     *         is permitted, and indicates that the cause is nonexistent or
     *         unknown.)
     * @since 1.5
     */
    public IllegalArgumentException(String message, Throwable cause) {
        super(message, cause);
    }

    /**
     * Constructs a new exception with the specified cause and a detail
     * message of <tt>(cause==null ? null : cause.toString())</tt> (which
     * typically contains the class and detail message of <tt>cause</tt>).
     * This constructor is useful for exceptions that are little more than
     * wrappers for other throwables (for example, {@link
     * java.security.PrivilegedActionException}).
     *
     * @param  cause the cause (which is saved for later retrieval by the
     *         {@link Throwable#getCause()} method).  (A <tt>null</tt> value is
     *         permitted, and indicates that the cause is nonexistent or
     *         unknown.)
     * @since  1.5
     */
    public IllegalArgumentException(Throwable cause) {
        super(cause);
    }

    private static final long serialVersionUID = -5365630128856068164L;
}
````

#### try and catch - checked exceptions handler
1. create own Extension class or go to Nr.2 when using an existing one.

``` JAVA
class KontoAusnahme extends Exception{
  /**
  * See JavaDoc for Exception
  */

  private static final long serialVersionUID = -551994305397592647L;

  public KontoAusnahme() {
  }

  public KontoAusnahme(String message) {
    super(message);
  }

}
```

2. add Exception triggers to a Method or class. For an Exception to trigger if it is not a Standard trigger (like int no String | Char -> InputMissmachtException) then the triggers have to be set up manually.
A list of Standard Exceptions :
https://gist.github.com/7yl4r/9e3f223f2eca76104940#file-reusableexceptions-md
Example below:

``` JAVA
public void zahleAus (double betrag) throws KontoAusnahme {
  if (betrag < 0) {
    throw new KontoAusnahme("Negativer Betrag: " + betrag);
  }
  if (saldo < betrag) {
    throw new KontoAusnahme ("Betrag > Saldo");
  }
  saldo -= betrag;
}
```
3. Wrap a try catch around where the method or class is used.

``` JAVA
class Test2 {
	public static void main(String[] args) throws KontoAusnahme {
		// Ausnahmen vom Typ KontoAusnahme werden weitergereicht
		// und führen zum Abbruch des Programms.
		Konto kto = null;
		try {
			kto = new Konto(1007, 500);
			kto.zahleAus(1000);
			kto.info();
		} catch (KontoAusnahme e) {
			System.out.println(e);
		}

		if (kto != null) {
			kto.info();
		}
	}
}  
```

4. the finally block - after the last catch block a finally block can be used for things that have to be done regardless of if a catch matches or not. Even when break, continue or return happens.

## Java classes
### How to design classes
1. Structure
  What information needs to be saved within the class? These are the **variables of the instance/class**.

2. behavior / dynamic
  What behavior needs to be represented? Or what operations needs to done? These are the **Methods of the class**.

Example properties of a bank account:
- an account has a **balance** (class variable)
- an account has an **interest rate** (class variable)
- **Money can be deposited** (to place money on the account -> Method)
- The **balance can be checked** (Method)
- The **interest rate will be applied** (Method)

### How to find classes? And how to name them?
Step by step quide from the school module 226-1_09:
1. Check the 'Requirements' (aka 'Anforderungen') of a program and look for nouns that have context to the program. Then decide for each noun (aka substantive) if it is relevant and could become a class or an attribute, if so highlight it.
2. List all highlighted nouns.
3. Pick 3-4 that are likly to be an instance / object in the running progra and write for each of those a 2-3 phrase behavior description. This does not have to be perfect but be willing to adjust it once possible with better knowledge.
4. If you need more than 2-3 sentences to describe the class, chances are your are not following the SRP, single responsibility principle.
5. For those picked find their attributes, write them down with data types. If this causes problem try to make think of things the class could have as an attribute from the list in under number 2.
6. Model the picks in a class model with attributes and data types. On paper or in Modelio.

### How to find methods?
double check not duplicate description?
This continues the guide from above:
7. go over your 'Requirements' again but this time look for verbs that relate to the found classes. Underline all those verbs.
8. then write them down and think about what paramter they might have.
9. model them in modelio or paper.
 
Voilà classes or attributes found and named.

What are good classes? //FIXME: Insert from: 'Was gute Klassen sind, wird am Ende des Arbeitsblattes zum Projekt Minesweeper (AB226A_09-1)!'


### Class description
``` JAVA
// Standard Class:
Modifiers returntype methodName (list ofParameters) [throws nameOfExceptions] {
  ClassVariables
  Constructors
  Methods
  }
```
#### Class Account (bank account) example:
##### private/public
``` Java
class Account {
  private double balance; // balance and interestRate are Instance variables
  private double interestRate; // set to private so only visible inside the object
}

...

public void deposit (double amount) {
  // the method to deposit is **public** because it should be reachable from outside of the class
  balance = balance + amount;
}
```

### Class Constructors
If any own Constructors are written the standard constructor disapears and needs to be added manually!!! See ex. below.

``` JAVA
// standard constructor
public Account() {};

// constructor with parameter accountnumber
// sets accountnumber to given number while building object
public Account(int accountnumber){
  this.accountnumber = accountnumber;
}

public Account(int accountnumber, int balance) {
  this.accountnumber = accountnumber;
  this.balance = balance;
}

// copy-Constructor copies object with account nr. a
public Account(Account a) {
  accountnumber = a.accountnumber;
  balance = a.balance;
}
```

### Object creation
ClassVariables of Account will be initialized with 0/null/false!

``` JAVA
Account myAccount; // creates variable
myAccount = new Account; // creates object and reference from variable to object

// or simply in one go...
Account myAccount = new Account;

// or with a constructor (accountnumber, balance)
Account myAccount = new Account(1000001, 1000);
```

### Acces to ClassVariables or Methods
How to access class Variables? This depends on the modifier of the variable (public/private). Generally said if the variable is private and can be changed use the method access. If it is public use the variable access.

``` JAVA
Account mA1 = new Account;
mA1.getSaldo(); // Point access to class Methods
mA1.interest = 0.02; // Point acces to ClassVariables

// more Ex.
class TestAccount {
  public static void main(String[] args) {
    Account account1 = new Account(); // =
    Type Variable = new Object();

    account1.deposit(100); // =
    variable.method(value);
  }
}
```

How to access or print the variables of an object of a class?
(The "Account" class has as a variable another class. This variable is called owner but is itself a class called "Customer", see code below). Lets say we want to print the name of the owner of the account. How to do this? Asuming the private label for both variables is correct (preventing the direct access and manipulation of the variables).

``` JAVA
package test;

public class ClassAndMethods {

	public static void main(String[] args) {

		Account a1 = new Account(new Customer("Foo"));

		// magic Answer on how to print Customer "Foo" not known jet....
		// something along: a1.getCustomer.name would be nice...
		//workaround or solution to this? write a get method as follows
		// (getName needs to exist in Customer!) in class Account:
		System.out.println(a1.getOwner()); // test.Customer@70dea4e

		Customer c1 = a1.getOwner();
		System.out.println(c1.getName()); // Foo

	}
}

class Account {
	private Customer owner;

	public Customer getOwner() {
		return owner;
	}

	public Account(Customer customer) {
		this.owner = customer;
	}
}

class Customer {
	private String name;

	public String getName() {
		return name;
	}

	public Customer(String string) {
		this.name = string;
	}
}

```

### Naming (convention of names)
* package names -> just small letters and numbers
* names for classes and interfaces start with a capital letter. Ex. MyClass
* names for variables start with small letters. Ex. myAccount
* names for Constants (variables with non changing values) just capital letters. Words are separated with underscore. Ex. KEY_FIRST
* Methods are named after verbs, start small. Ex. calculate, drawFigure

### Standard classes
#### The String class
What is a String? A chain of Characters made out of Unicode. This chain is made with an char[] Array containing the String.
What are literals? "Hallo" is a literal.
After initalizing the "Hallo" becomes an object. ->
String s = "Hallo";
Strings are immutable.

What is a good way to learn more about the String class?
1. Read the Javadoc String entry: https://docs.oracle.com/javase/8/docs/api/java/lang/String.html Same entry in Eclipse when hovering over expression 'String();'
2. In Eclipse make a .java classfile, write String(); then hit F3 on this  which takes you to the String Constructor Code Origin and all its deviations.

Easy **concatenation** with +.

**Conversion** of String with toString which is an Object class Method, therefore every Class inherits it.

Java recommends for **further read** into string concatenation and conversion: https://docs.oracle.com/javase/specs/ see Gosling, Joy, and Steele, The Java Language Specification

#### Formatting String output and escape Sequences
https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html#syntax
Instead of 'Syso("Text\n")' better use 'Syso("Text%n")'

Good Example use of 'syso("Print String here: %s")': https://docs.oracle.com/javase/tutorial/java/data/numberformat.html

List of options as escape sequences
https://docs.oracle.com/javase/tutorial/java/data/characters.html

### String Examples for equals
.equals gives true when the same sequence of characters is in both objects. Where as == gives true when the reference is the same.
``` JAVA
//		compare		
		String s = "Hallo";
		String t = new String(s);
		String u = "Hallo";

		System.out.println(s.equals(t)); // true
		System.out.println(s == t); // false -> this says it has not the same reference in memory
		System.out.println(s.equals(u)); // true
		System.out.println(s == u); // true

```

### Methods
A Method consist of a Method Head and Body.

``` JAVA
[Modifiers] ReturnType Method-name ([parameters]) [throws exception list] {
  methodDo Code;
  return variable; // Only if required.
}
```

#### Methods Return types
Method can give back a value. The value can be a data type or reference type (class). A Method without a return value has the type void.

Values are returned with the statement:

``` JAVA
return expression; // Ex. return sum, return name, etc.
```
The return statement ends (exits) the method!

#### Methods parameters
The parameters (type name) specify the arguments, which are give to the method when calling her. parameters are separated by commas.

Parameters can be called by value or by reference.

#### Methods local variables
Variables are local in a Method when created, used and discarded within one method. While in the Method variables outside the method or block with the same are hidden!

Final local variable = constant; initialized with value then can not be reasigned within the method.

#### Methods this reference
this means the actual object. The this is used to distinguish the one class attriute with the same name form the method attribute or parameter with the same name. Ex.

``` JAVA
pubic void setAccountnumber (int Accountnumber) {
  this.Accountnumber = Accountnumber;
}
```

#### Method overloading
Simply put this means that a method with the same name can handle different inputs. How? By having the same method with defined with different parameters. Ex.:

``` JAVA
public int max(int a, int b) {
  return a < b ? b : a;
}

public double max(double a, double b) {
  return a < b ? b; a;
}

public int max(int a, int b, int c) {
  return max(max(a,b), c);
}
```

#### Method usage from outside of a class
If you want to use a method from another class, the class needs to be initialize first! otherwise it will throw a 'java.lang.NullPointerException'.

### Static Attribute or operations, what about static classes?
Static for a variable makes sense if the variable is the same for all instances of the class. Ex. if for Students there is only one school this school would be static. Or for a counter the counter has to be the same for all instances then it should be static. NOTE: In one Example, TestgetriebeneEntwicklung, they told me to use two static variables bound to two constructor calls within the own class. These where creating two class instances and setting the class variables to different values. But the example suggested that these variables be static. Which made the 2nd Instance overwrite the first. It took me a while to figure this out.

Nonstatic variable:
``` JAVA
int count=0;//will get memory when instance is created  

Counter(){  
count++;  
System.out.println(count);  
}  

public static void main(String[] args) {
  Counter c1 = new Counter(); // Output will be 1
  Counter c2 = new Counter(); // Output will be 1
}
```

Static variable:
``` JAVA
static int count=0;//will get memory only once and retain its value  

Counter(){  
count++;  
System.out.println(count);  
}  

public static void main(String[] args) {
  Counter c1 = new Counter(); // Output will be 1
  Counter c2 = new Counter(); // Output will be 2
}
```
#### Static Methods
If you apply static keyword with any method, it is known as static method.

- A static method belongs to the class rather than object of a class.
- A static method can be invoked without the need for creating an instance of a class. (Ex. Main Method, instead of creating instance it is invoked directly...)
- static method can access static data member and can change the value of it.

### enum class
Enumeration class is used to create a list of constant objects with the same custom type.

- enum = enumerated types
- fixed list of possible values
- specified when the enum is created

Basic enum class:
``` JAVA
enum EnumTypeName {EnumList}
```

Ex. enum class Trafficlightcolor or Season:
``` JAVA
public enum Trafficlightcolor {
  RED, YELLOW, GREEN
}

enum Season { SPRING, SUMMER, FALL, WINTER }
```

By convention, enum values are given names that are made up of upper case letters, but that is a style guideline and not a syntax rule. An enum value is a constant; that is, it represents a fixed value that cannot be changed. The possible values of an enum type are usually referred to as enum constants.

Note that the enum constants of type Season are considered to be "contained in" Season, which means -- following the convention that compound identifiers are used for things that are contained in other things -- the names that you actually use in your program to refer to them are Season.SPRING, Season.SUMMER, Season.FALL, and Season.WINTER.

Enum contants can be used in switch statements as triggers.

Enum would be great for the pins in Mastermind.

### JAVA switch statement
https://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html. Instead of doing a lot of if and else there is the switch statement for a multi option fork. Example input can be 1-5, each input leads to another operation -> use switch statement.

``` Java
public class SwitchDemo {
    public static void main(String[] args) {

        int month = 8;
        String monthString;
        switch (month) {
            case 1:  monthString = "January";
                     break;
            case 2:  monthString = "February";
                     break;
            case 3:  monthString = "March";
                     break;
            case 4:  monthString = "April";
                     break;
            case 5:  monthString = "May";
                     break;
            case 6:  monthString = "June";
                     break;
            case 7:  monthString = "July";
                     break;
            case 8:  monthString = "August";
                     break;
            case 9:  monthString = "September";
                     break;
            case 10: monthString = "October";
                     break;
            case 11: monthString = "November";
                     break;
            case 12: monthString = "December";
                     break;
            default: monthString = "Invalid month";
                     break;
        }
        System.out.println(monthString);
    }
}
```

#### Return in Switch

``` JAVA
private static int calc(int operand1, int operand2, int operator) {
  int result = 0;
  switch (operator) {
  case 1:
    result = (operand1 + operand2);			
    break;
  case 2:
    result = (operand1 - operand2);			
    break;
  case 3:
    result = (operand1 * operand2);			
    break;
  case 4:
    result = (operand1 / operand2);			
    break;
  case 5:
    result = (operand1 % operand2);			
    break;
  default:
    System.out.println("Operator ungueltig.");
    break;
  }
  return result;
}
```
Side Note: instead of break you could use return as far I tried it worked #recheckit

## Java Input and Output or I/O
This is to capture some of the knowledge gained using I/O in own programs, since this topic is pretty complex.

Some base classes are provided in java.io check them out. In my attemps so far I mostly used the Scanner which is java.util based. So keep in mind there are diffrent ways to get the user input from console.The interessting part is that the 3 methods to get user input are very different and have pros and cons and evolved along the Java versions. You might wanna see 3 examples from: http://www.codejava.net/java-se/file-io/3-ways-for-reading-input-from-the-user-in-the-console

### Reading Input from Console
#### Other way to be documented
#### Using Console Class
Read userInput with 'System.Console.readLine()'. Note the example below is written as a JUnit test since Console class does not work live in an IDE console representation. To get a live input via console the System.console.readLine() method code needs a real CLI open, like CMD in Windows or terminal in Unix. Otherwise it will default to NullPointerExecption since it expects to find a active console.

Some other thoughts on the Console class, and a way around when testing without an active console: http://illegalargumentexception.blogspot.ch/2010/09/java-systemconsole-ides-and-testing.html
and the official jdk9 https://docs.oracle.com/javase/9/docs/api/java/io/Console.html

**So it seems this would be the way to go, but it is the most difficult way to implement as a beginner.**

**This test does not work!** See Eclipse Console output will be: "No console: non-interactive mode!". Why? Since JVM can't find an active console it will exit.
How to fix this? This would need an abstraction and a switch when no console present use another input reading method. See Link above from illegalargumentexception.blogspot.ch
''' Java
@Test
@DisplayName("testUseSystemConsoleOLD: learn NullPointerExecption has to be handeld with System.console.readLine()!")
void testUseSystemConsoleOLD() {

	// Console console = System.console(); // without 'if (console == null)' -> NullPointerExecption

	Console console = System.console();
	if (console == null) {
		System.out.println("No console: non-interactive mode!");
		System.exit(0);
	}

	System.out.print("Enter your username: ");
	String username = console.readLine("TestUserName"); // This is where the readLine would actually get the User input

	assertEquals("TestUserName", username);
}
'''

## JavaDoc
Insert know from chapter 12 ASAP!

## Java, compile and run, in CLI mode and on what happens when loading a Java file
### Compile a Java file to binary
$javac myJavaFile.java
Results in myJavaFile.class generated where the .java file is. Thi implies you need to be in the folder of the .java file. See help section of JVM for other destination.

### Run a binary file aka class file
$java myJavaBinary
Note the file needs to be a class aka filename.class file.

### Java CLI commands not working?
Check your PATH file, links to correct JDK version? See poweshell.mdf for more on PATH. After setting this reboot?
On the 'could not find main' error see official: https://docs.oracle.com/javase/tutorial/essential/environment/paths.html
Also good read for CLASSPATH https://en.wikipedia.org/wiki/Classpath_(Java) or https://de.wikipedia.org/wiki/Klassenpfad
Apart from PATH, OS variable to find program commands (.exe etc.), there is also the need for CLASSPATH, OS variable to find Java classes if not in the same directory. This problem can be avoided running the .jar file. More on this topic, how to run programs, see eclipse.md. In short creating a runnable .jar the whole setup is done by Eclipse.

### Try and Error
If the .java file does not have a package tag in it and does not import any other stuff and both .java and .class file are in the same directory. Run the program with '$java myMainfileName'. Note don't specify the .class or .java file just go with the plain Filename.
If the .java file only has an import tag then it might depend what import it is according to Classloader see blow (standard import ok, custom might need path).
Own test shows import of java.io.Console work on the fly but having a package declared in the .java file needs CLASSPATH correction (manually delete the package reference if not needed or really learn how to use CLASSPATH command or use Eclipse to make a .jar (fixes CLASSPATH issues for you)).

### JAR file?
Java Archive, is a zipped file containing .java, .class files and all needed libraries? roughly...

### What happens when loading a .jar file or running $ java myProgram?
This is well explained in the Java Classloader https://en.wikipedia.org/wiki/Java_Classloader
Basically some parts or classes are loaded automatically some are not.