# JUnit - How to test with JUnit - My notes
Order of these Notes is based on knowledge, Basic knowledge on top, each new topic builds up on the prior one. This is for simplicity and keep it easy to make new notes.

## What is JUnit?
A java framework, to test java code. The framework is written in java. The framework can be used for existing code but also allows to write code with the 'extreme programming' approach. Meaning to write the tests before writting the code. Aka think about the class, then write the ClassTest with the test for the methods first, then write the actual Class. Also this makes thinking about classes with their testablility in mind first, that those tests will be easier to run. Imagine instead writting the test cases after all the code exists, it's a lot harder to figure out all the needed test cases.

## JUnit 5 - Junit.org
* Junit = Java framework to write and run automatet unit tests.
* Units = methods, classes or components
* Junit5 = Java8

### JUnit.org website content
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

### JUnit framework setup
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

### JUnit and Eclipse
Junit user guide links to: https://www.eclipse.org/eclipse/news/4.7.1a/#junit-5-support
* Junit Jupiter is already part of the Eclipse Java EE
* **to create a JUnit test case**, select the class to make test class of, right click, say new > Junit test case > select methods to create, add Junit lib to project, write code. More info see link above.

## JUnit Syntax
Example of a Testclass. Naming convention ClassnameTest.

``` java
public class ApartementTest {
  private Renter renter = new renter ("Hugo", "Hungerbühler", "12345");
  private Apartment apartment;

  //before each testrun
  @Before
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

## Testing based on: Intuition, create equivalent classes or boundaries (Intuition, Aequivalenzklassenbildung oder Grenzwertanalyse)
Explanation to these terms:
* Intuition - the test methods are written mostly on the experience of the tester where he thinks problems and errors will occur in the code.
* equivalent classes - imagine  a black box, the program. It has input and output. We look at the results of the output according to the input. All the values that accomplish the same output can by grouped in an equivalent class. According to the statement: 'Testing of an equivalent class value, equals testing of all the equivalent class values, exept the boundaries'. Often equivalent classes are divided in valid and invalid ones: customernumber is valid from 1000 to 9999. valid equivalent class 1000-9999, invalid equivalent class >1000 and <9999
* boundaries - testing is focused on the boundaries of values, the three tests to make are inside, outside and at the boundaries. Are the results as expected.

## Junit with eclipse
### write testcase first then autogenerate class
As shown in the example of F. Westfahl, http://www.frankwestphal.de/UnitTestingmitJUnit.html, write the tests first. The code can then be generated from that with ease.

* Generate eclipse project
* Generate new Junit test case via new wizzard
* write test case
* generate class and method from testcase
* write method in class
* run testclass
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
