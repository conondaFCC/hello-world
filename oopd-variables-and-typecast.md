#oopd-variables-and-typecast
## the implications with variables and Super class and sub classes
### typecast example, how the information is not lost referencing over a super class

``` Java
// typecast syntax:
subclassVariable = (typecastSubClass) supperClassVariable;

// code example:
Chef chef1 = new Chef("Meier", "Hans", 1, "Einkauf"); // create sub class object
	Person p1; // create Super class reference
	p1 = chef1; // set reference to sub class object
	
	Chef chef2; // create new sub class reference
	chef2 = (Chef) p1; // reference to the same reference of p1, with typecast
	System.out.println(chef2.getAbteilung()); //prints "Einkauf" // the information of the first object can be accessed
```

Code explanation:
What this shows is best understood if you know how Java will represent this in memory.

The first line creates a reference chef1:Chef, which will point to a Chef-object. What is hidden in the code is Chef extends Person, so it is a child of Person. So the Chef-object contains a Person-object with those attributes. Actually the paraeters 1-3, are Superclass parameters, only the 4th is for the Chef class. So all parameters set all values for the attributes.

The 2nd line then creates a reference p1:Person.
The 3rd line references the p1 to the same object as the reference chef1. But since p1 is of type Person, it can not link to the subclass Chef but will link to the super class Person of the same object!

The 5th line creates a new refererence chef2:Chef.

The 6th line sets the reference of chef2 to the same object like p1 but with a typecast. Without the typecast this reference would be invalid, because a sub class can not be referenced to a super class. For the typecast to work the object outside the super class needs to be of the same datatype, in this example 'Chef', otherwise this will result in a runtime error. 

The 7th line shows that it is possible to call the attributes of the sub class again!

### Declaration of Variables example
see also @oopd-declaration-of-attributes.md
Variables on Super class should be set to protected to facilitate the use form subclasses. Can be set to private if need be.
Variables on sub class should be set to private to fullfil encapsulation rules. This means can only be accessed via get, set methods.

Contructors with Super class and sub class
A nice way to make the constructors for sub classes is to use the contructor of the Super class first, then add the sub class variables.

Note once a contructor has been set for the super class, any child will have to use this constructor, since the default constructor, the empty one will not be built automaticly anymore. Either use the new constructor or add the default constructor manually. The former is preferable.
See Example:
``` Java

class Person {
	protected String name;
	protected String vorname;
	protected int personalNummer;

	public Person(String name, String vorname, int personalNummer) {
		this.name = name;
		this.vorname = vorname;
		this.personalNummer = personalNummer;
	}

}

class Chef extends Person {
	private String abteilung;

	public Chef(String name, String vorname, int personalNummer, String abteilung) {
		super(name, vorname, personalNummer);
		this.setAbteilung(abteilung);
	}

	/**
	 * @return the abteilung
	 */
	public String getAbteilung() {
		return abteilung;
	}

	/**
	 * @param abteilung the abteilung to set
	 */
	public void setAbteilung(String abteilung) {
		this.abteilung = abteilung;
	}
}

class Fachangestellter extends Person {
	private Chef vorgesetzter;

	public Fachangestellter(String name, String vorname, int personalNummer, Chef vorgesetzter) {
		super(name, vorname, personalNummer);
		this.setVorgesetzter(vorgesetzter);
	}

	/**
	 * @return the vorgesetzter
	 */
	public Chef getVorgesetzter() {
		return vorgesetzter;
	}

	/**
	 * @param vorgesetzter the vorgesetzter to set
	 */
	public void setVorgesetzter(Chef vorgesetzter) {
		this.vorgesetzter = vorgesetzter;
	}

}
```
