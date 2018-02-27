# JUnit - How to test with JUnit - My notes
Order of these Notes is based on knowledge, Basic knowledge on top, each new topic builds up on the prior one. This is for simplicity and keep it easy to make new notes.

## What is JUnit?
A java framework, to test java code. The framework is written in java. The framework can be used for existing code but also allows to write code with the 'extreme programming' approach. Meaning to write the tests before writting the code. Aka think about the class, then write the ClassTest with the test for the methods first, then write the actual Class. Also this makes thinking about classes with their testablility in mind first, that those tests will be easier to run. Imagine instead writting the test cases after all the code exists, it's a lot harder to figure out all the needed test cases.

### JUnit quick references
* One good quick summary of JUnit as a refresher with most commands and basic workflow (7 pages in German, PDF) https://homepages.thm.de/~hg11260/mat/junit.pdf

### JUnit 5 - Junit.org
* Junit = Java framework to write and run automatet unit tests.
* Units = methods, classes or components
* Junit5 = Java8

#### JUnit.org website content
```
      JUnit.org
        |
    --------------------------
    |         |     |         |
userguide   git   Javadoc   Q&A
```
* userguide = reference guide, user documentation for junit (ide support for intelliJ IDEA and Eclipse (under run) or custom build for other IDE, test examples and much more)
* git = repo
* Javadoc = see yourself
* Q&A = come here for common question

#### JUnit framework setup
```
      JUnit
        |
--------------------------
|           |             |    
platform   jupiter      vintage
```
* all are a part or contain: testframework, testengine
* platform: links to JVM, Maven | Gradle
* **jupiter**: write and run **Junit5**
* vintage: run Junit3+4 code

## JUnit Syntax
Example of a Testclass. Naming convention ClassnameTest.

``` java
public class ApartementTest {
  private Renter renter = new renter ("Hugo", "Hungerbühler", "12345");
  private Apartment apartment;

  //before each testrun
  @BeforeEach
  public void setUp() {
    apartment = new Apartment (renter, 1035.50);
  }

  @Test
  public void testRiseRent() {
    assertEquals(1035.5, wohnung.getRent(), 0);
    wohnung.riseRent(100);
    assertEquals(1135.5, wohnung.getRent(), 0);
    wohnung.riseRent(100);
    assertEquals(1235.5, wohnung.getRent(), 0);
  }
…
}

```

## How to write test cases?
There are different approches on how to write test cases. It is not obvious how detailed test cases should be (expecially on projects where code exists, in contrast to starting a new project based on the principle test first, then write code). I try to describe what is needed in writing tests as detailed as I can. On the bases of examples and experiance.
* One approch is to **test every function the program has to do**. This is a **good approach when no code exists**, as described by the good introduction from F. Westfahl (Junit example with no prior code http://www.frankwestphal.de/XPueberdieSchultergeschaut.html ). 
* If **code already exists another option might be write test cases similar to use cases** where the user does or wants something so the result is clear and can be tested. For example the user input will be tested, this means the constructor of the class which tests the user input can be tested. Lets say the class was called 'InputValidator', this would give us the test of the constructor and its functions in test cases called: testInputValidatorCorrectEntry() as one test, and other cases for testInputValidatorTooLongEntry(), ...TooShortEntry(),...WrongCharacters(),... etc.

### Testing based on: Intuition, create equivalent classes or boundaries (Intuition, Aequivalenzklassenbildung oder Grenzwertanalyse)
Explanation to these terms:
* Intuition - the test methods are written mostly on the experience of the tester where he thinks problems and errors will occur in the code.
* equivalent classes - imagine  a black box, the program. It has input and output. We look at the results of the output according to the input. All the values that accomplish the same output can by grouped in an equivalent class. According to the statement: 'Testing of an equivalent class value, equals testing of all the equivalent class values, exept the boundaries'. Often equivalent classes are divided in valid and invalid ones: customernumber is valid from 1000 to 9999. valid equivalent class 1000-9999, invalid equivalent class >1000 and <9999
* boundaries - testing is focused on the boundaries of values, the three tests to make are inside, outside and at the boundaries. Are the results as expected.

### more on equivalent class testing
For example we want to test the Mastermind game color options, the test would be Test(classname)correctInput {}, if we have 6 colors but only 4 at one time can be put in then we don't need to test every combination. But we should test at least every color of those six in at least one combination is valid. So we would make two test's to check the 6 colors so every color is tested once. Another example of this would be if the clientnumber from the example above where the equivalten class was first introduced. Say the clientnumber runs from 1-9999, then it would be good not only to check one number but maybe every digit combination from 1-4, like (4,32,546,7345) and make sure those four combinations are valid. Does the logic come across?

## JUnit and Eclipse
Junit user guide links to: https://www.eclipse.org/eclipse/news/4.7.1a/#junit-5-support
* Junit Jupiter is already part of the Eclipse Java EE
* if no previous code exists, which is the standard way to go, when writing code in the extreme programming style, create a new class "JUnit test case" and write tests first. Explained in more detail below.
* **to create a JUnit test case**, select the class to make test class of, right click, say new > Junit test case > select methods to create, add Junit lib to project, write code. More info see link above.
* best way to import JUnit5 lib, create testclass with @Test, Ctrl+1 over @Test, import will be suggested.

### workflow for JUnit test in Eclpise
#### write testcase first, then autogenerate class and methods, then run test
Ideal approch to new code and projects. As shown in the example of F. Westfahl, http://www.frankwestphal.de/UnitTestingmitJUnit.html , write the tests first. The code can then be generated from that with ease.

* Generate eclipse project
* Generate new Junit test case via new wizzard
* write test case
* generate class and method from testcase
* write method behaviour in class
* run testclass as JUnit test (via Run as command)
* iterate for new classes and methods

Example of testcase:
``` Java
package mod226_08;

import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.Test;

class EuroTest {

	@Test
	public void testAmount() {
		Euro two = new Euro(2.00);
		assertTrue(2.00 == two.getAmount());
	}
}
```
At this point the class Euro and the method getAmount() was not made, but can be made via autosuggestion very quickly.
''' Java
public class Euro {
  private double amount;

  public Euro(double amount) {
    this.amount = amount; // almost all that needs to written by hand
  }

  public double getAmount() {
    return this.amount;// almost all that needs to written by hand
  }
}
'''

## Annotations and Assert methods
Excerpts only!
### Annotations
For classes:
@RunWith(... .class) // use with SuiteClasses
@Suite.SuiteClasses({...
.class, ...}) // used to test multiple classes at the same time, see the example chapter for details

For methods:
@Test // defines a test to be run
@Test(expected=. . . Exception.class) // test fails when other exeption is thrown than defined
@Test(timeout =. . . ms ) // test fails when taking longer as defined
@Ignore("comment ") // will disable method, give reason in comment

### Assert methods
assertEquals(Object exp, Object act)
assertEquals(float exp, float act,
float delta)
assertFalse(boolean condition)
assertNotNull(Object object)
assertNotSame(Object unexp, Object act)
assertNull(Object object)
assertSame(Object exp, Object act)
assertTrue(boolean condition)

## Examples of JUnit test cases
### Example of WohnungTest and the use of @BeforeEach|BeforeAll
''' Java
public class WohnungTest {
  private Mieter mieter = new Mieter ("Hugo", "Hungerbühler", "12345");
  private Wohnung wohnung;

  //vor jedem Testlauf:
  @BeforeEach
  public void setUp() {
    wohnung = new Wohnung (mieter, 1035.50);
  }
  
  @Test
  public void testErhoeheMiete() {
    assertEquals(1035.5, wohnung.getMiete(), 0);
    wohnung.erhoeheMiete(100);
    assertEquals(1135.5, wohnung.getMiete(), 0);
    wohnung.erhoeheMiete(100);
    assertEquals(1235.5, wohnung.getMiete(), 0);
  }
…
}

// created class Euro and its methods
public class Euro {
  private double amount;

  public Euro(double amount) {
    this.amount = amount;
  }

  public double getAmount() {
    return this.amount;
  }
}
'''

''' Java
public class Wohnung {
	private Mieter bewohner;
	private double miete;
	private double nebenkosten;
	
	public Wohnung(Mieter bewohner, double miete) {
		this. bewohner = bewohner;
		this. miete = miete;
	}
	
	public void erhoeheMiete(double betrag) {
		miete += betrag;
	}
	
	public double getMiete() {
		return miete;
	}
}
'''

### SuiteClasses, testing multiple classes at the same time
''' Java
import org.junit.runners.Suite;
import org.junit.runners.Suite.SuiteClasses;
import org.junit.runner.RunWith;

@RunWith(Suite.class)
@SuiteClasses( {
	EuroTest.class,
	//list all test classes comma separated
})
public class AllTest {
}
'''
