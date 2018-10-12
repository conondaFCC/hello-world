# JavaDoc Guide
## Why document program code?
- After a while the person who has written will forget why and what he has programmed in a specific fashion.
- For someone else to understand the code in a reasonable time

## What is JavaDoc?
JavaDoc is a program that runs over .java files and finds JavaDoc comment and will create a navigable html site out of it. To run it run javadoc.exe from the JDK. It is not inside JRE! More Details see How to write JavaDoc, chapters.
JavaDoc comment is always before the block it will describe. JavaDoc uses tags to specify certain types like parameters, author etc.

## How to write JavaDoc comments
The syntax of JavaDoc can best be seen in the subchapter 'Example of JavaDoc comment in the class Validator'.

### What should be documented as JavaDoc comment and how?
**General rule, every class and public method should have a JavaDoc comment.** Exeptions: trivial classes, getter and setters, simple constructors. A class or method comment, describes briefly what the task is, what is does. To get the a good class description it migth be good to start with the methods first to get a grasp for what the class does, then describe the class.

#### For methods
Return values should be explained, like what does the return stand for, what is its goal or purpose.
Exceptions might need explanation.
Parameters, describe what they are, thier goal and usage if not clear, especially mention it, when the allowed range is reduced!
Instance variables - it can help to understand the code to describe even private instance variables.

### How to generate JavaDoc HTML
There are two ways, via Eclipse and with the JDK javadoc.exe. I recommend the Eclipse way since I don't know how to use javadoc.exe.
For Eclipse the steps are rather simple. Go to your project > in the menu ribon select project > generate JavaDoc. A wizard will guide you through. Just select what packages or classes your Javadoc should contain. Last Question is set a JavaDoc standard for this package, which can be denied and the JavaDoc will still generate.

### List of common JavaDoc tags
Tag, param  	| Description			| Use
------------	|----------				|---
@author name	|Name ex. (Marfaing V.)	|class, interface
@version v-nr.	|once per class only! version javadoc = sw dev. ver???	|class, interface
@see reference	|Links to other element	|class, interface, instance variable, method
@param name		|desc of parameter		|method
@return			|desc of  return		|method
@exception		|learn usage first		|method
@throws			|learn usage first		|method

### Example of JavaDoc comment in the class Scanner
This is a JDK/JRE example class! The JavaDoc for the scanner class is like 250 lines long!
It describes the class function in one sentence. Then goes into more details how it does it in 4 more lines. Followed by examples. I just copied in the first few lines. Another thing I found is sometimes it will wrap several methods in one JavaDoc comment block.

´´´ Java
/**
 * A simple text scanner which can parse primitive types and strings using
 * regular expressions.
 *
 * <p>A <code>Scanner</code> breaks its input into tokens using a
 * delimiter pattern, which by default matches whitespace. The resulting
 * tokens may then be converted into values of different types using the
 * various <tt>next</tt> methods.
 *
 * <p>For example, this code allows a user to read a number from
 * <tt>System.in</tt>:
 * <blockquote><pre>{@code
 *     Scanner sc = new Scanner(System.in);
 *     int i = sc.nextInt();
 * }</pre></blockquote>
 * ...
 */
 
  /**
   * Fields and methods to match bytes, shorts, ints, and longs
   */
    private Pattern integerPattern;
    private String digits = "0123456789abcdefghijklmnopqrstuvwxyz";
    private String non0Digit = "[\\p{javaDigit}&&[^0]]";
    private int SIMPLE_GROUP_INDEX = 5;
    private String buildIntegerPatternString() {...}   
´´´ 

### Example of JavaDoc comment in the class Validator
´´´ Java
/**
 *  Validator class break up the input in parts,
 *  validates the parts, (commandchar, line, row)
 *  and makes the split input available.
 *
 *  @author V. Marfaing
 *  @version 1.2
 */

public class Validator {
  private String[] splitInput;
  private boolean isValid = true;


  /**
   * Constructor. Splits the input then checks if part1 is a valid char, are
   * and part2-3 are numbers.
   * @param input String from user.
   */

  public Validator(String input) {
    splitInput = input.split(" ");
    if (splitInput.length != 3) {
      isValid = false;
      return;
    }
    if (!splitInput[0].equals("T") || !splitInput[0].equals("M")) {
      isValid = false;
      return;
    }
    try {
      Integer.valueOf(splitInput[1]);
      Integer.valueOf(splitInput[2]);
    } catch (NumberFormatException e) {
      isValid = false;
    }
  }

  /**
   * Holds true if input is valid.
   * @return validity of input
   */

  public boolean isValid() {
    return isValid;
  }

  /**
   * takes the splitInput and creates a command object out of it.
   * @return command object with splitInput
   */
  public Comando createComando() {
    String comandChar = splitInput[0];
    int line = Integer.valueOf(splitInput[1]);
    int row = Integer.valueOf(splitInput[2]);
    return new Comando(comandChar, line, row);
  }
}
´´´

