# test getriebeneentwicklung
Zusammenfassung des Buches von Frank Westphal.

## Einleitung: Sinn der testgetriebenen Entwicklung
Um die funktionale und die strukturelle Qualität aufrechtzuerhalten. Funktionale Qualität meint die Benutzbarkeit und Fehlerfreiheit, strukturelle Qualität meint das Design und die Codestruktur für die Weiterentwicklung. Die gesamte Qualität ist nicht die Summe der beiden Teile, sondern wie in einer Kette nur der schwächste Teil.
* Zielgerichtetes Entwicklen. Die Test geben den Weg vor.
* Sicheres Entwicklen. Jeder Programmteil ist überprüfbar und bleibt es auch bei Veränderungen am Code.
* Schrittweise Entwicklen. Kleine Schritte führen schneller zu einem vorzeigbaren ganzen, zu jederzeit.
* Nach jedem Tag ist die Menge neuer Test ein Gradmesser für das foranschreiten der Entwicklung

### Inahlt des Buches, Kap. 1.3 beschreibt alle Kapitel
* Testgetriebene Entwicklungen, im Kleinen und Grossen (Unit Tests und Akzeptanztests)
* Kap 2 - Phylosophie und Basistechniken
* Kap 3 - JUnit
* Kap 4 - Testgetriebener Entwicklungszyklus
* Kap 5 - Refactoring
* Kap 6 - Integration im Team
* Kap 7 - fortgeschrittene Techniken
* Kap 8 - meistern schwierige Testsituationen
* Kap 9 - alternative zu Kap 8
* Kap 10 - FIT
* Kap 11 - Schlussgedanken

### Codebeispiele nicht nur lesen sondern nachvollziehen
Die Codebeispiele im Buch sollten am besten selber nachgeschrieben und getestet werden.
Bsp. sind auch auf der Webseite vorhanden:
* http://www.frankwestphal.de/
* http://www.TestgetriebeneEntwicklungmitJUnitundFIT.html

### Konkrete Teststrategien finden
Buch von Johannes Link [li05]

## Kap 2 - Phylosophie und Basistechniken
Wie bereits in JUnit.md beschrieben beginnt die Testgetriebene Entwicklung mit einem Test-Framework welches zur verwendeten Entwicklungsumgebung eingesetzt werden kann. Danach wird eine neue Funktionalität erst als Test geschrieben und dann der Code zum Testfall geschrieben. Damit wird nach und nach das ganze Program mit  alles umfassenden Testreihen aufgebaut, welche es erlauben die Funktionalität aller Teile zu garantieren.

### Beispiel Videoverleih
Beginnt mit den Annahmen: Kunden leihen Videos aus, Kostet am für die ersten zwei Tage 2.-, ab dem 3. Tag 1.75 pro Tag. Klasse ableiten: Customer -> Testklasse erstellen CustomerTest als JUnit Test case in Eclipse.
Für jede Methode eine Zusicherung schreiben um die Funktion abzusichern. > Mittels einer Assert-Methode
''' Java
@Test // Test 1
	void testRentingOneMovie() {
		customer.rentMovie(1); Funktion Filmausleihen
		assertTrue(customer.getTotalCharge() == 2); //Funktion Totalkosten
	}
'''

#### Designstrategie = Einfaches Design
Einfach anfangen und dann verbessern und hinzufügen. Kompliziertes aufschieben. Nur programmieren was auch benötigt wird (vom Test vorgegeben). Extremfall in Lösung 1, als Return Wert wird 2 zurückgegeben!!! Test erfüllt!
''' Java
// Lösung 1
public class Customer {
	public void rentMovie(int daysRented) {	}
	public int getTotalCharge() {
		return 2; // Beachte die einfache Lösung!!!
	}...
'''

Ausleihe von zwei Filmen testen, Test schreiben und Test laufen lassen auch wenn Intuition sagt das der Test fehlschlägt!
''' Java
@Test // Test2: 
	void testRentingTwoMovies() {
		customer.rentMovie(1); // Ausleihe ein Tag
		customer.rentMovie(2); // Ausleihe zwei Tage
		assertEquals(4, customer.getTotalCharge());}
		
// Code in Klasse Customer:
	private int totalCharge = 0;

	public void rentMovie(int daysRented) {
		totalCharge = totalCharge + 2;
	}

	public int getTotalCharge() {
		return totalCharge;
'''

#### Evolutionäres Design
Dritten Testfall hinzufügen. Ein Film 3 Tage ausleihen.

''' Java
	@Test // Test 3!
	void testRentingThreeMoviesMoreThanTwoDays() {
		Customer customer = new Customer(); //Instanz mit Customerkonstruktor erstellen
		customer.rentMovie(1); // Ausleihe ein Tag
		customer.rentMovie(2); // Ausleihe zwei Tage
		customer.rentMovie(3); // Ausleihe drei Tage (2+1.75=3.75)
		assertEquals(7.75, customer.getTotalCharge(), 0.001);
		// 0.001 ist die Toleranz bei Fliesskommazahlen
	}
'''

#### Abschluss Programmierepisode
Bewusst aufhören ist eine Stärke. Wenn alle Bedingungen getestet sind. Es muss nicht für jede Methode ein Test geschrieben werden. Nur für die welche Probleme verursachen könnten. **Das Wissen zum Code wird in den Tests festgehalten.** Das vorgenommene wurde erreicht wenn allte Tests erfüllt sind.
''' Java
	// letzter Test mit vier Filmen 
	@Test
	void testRentingFourMovies()  {
		Customer customer = new Customer();
		customer.rentMovie(1); // Ausleihe ein Tag
		customer.rentMovie(2); // Ausleihe zwei Tage
		customer.rentMovie(3);
		customer.rentMovie(4); // Ausleihe vier Tage
		assertEquals(13.25, customer.getTotalCharge(), 0.001);
	}
		
	public void rentMovie(int daysRented) {
		totalCharge = totalCharge + 2;
		if (daysRented > 2) {
			totalCharge = totalCharge + (daysRented -2) * 1.75;
		}
	}	
'''

#### Refactoring
Der erste Versuch ist nie das Endergebnis. Also fliessen die Erfahrungen beim erstellen wieder zurück in das Design. Dieser Prozess heisst Refactoring. Ein schrittweises Wachstum und ständige Überarbeitung. Nebenbei die Redundanz beseitigen. Im Beispiel wird versucht den Code verständlicher zu machen. Was bedeuten all die Zahlen? Durch die neuen Namen für die Konstanten passen diese aber nun nicht mehr zur Klasse Customer, deshalb wird nach dem Funktionstest, eine neue Klasse Movie erstellt, d.h. zuerst die Tests dazu. Dann die Funktion aus der Klasse Customer in die Movie Klasse. Und die Customer Klasse anpassen. **Die alten Test der CustomerTest Klasse laufen weiterhin fehlerfrei!**

''' Java
// In der Klasse MovieTest werden die vier Szenarien Ausleihen nochmal getestet ob diese die richtigen Werte zurückgeben wie beim Test Customer.
class MovieTest {
	@Test
	void testBasePrice() {
		assertEquals(2.00, Movie.getCharge(1), 0.001);
		assertEquals(2.00, Movie.getCharge(2), 0.001);
	}

	@Test
	void testPricePerDay() {
		assertEquals(3.75, Movie.getCharge(3), 0.001);
		assertEquals(5.50, Movie.getCharge(4), 0.001);
	}
}

// Die Berechnung findet neu in der Klasse Movie statt:
public class Movie {
	private static final double BASE_PRICE = 2.00; // Euro
	private static final double PRICE_PER_DAY = 1.75; // Euro
	private static final int DAYS_DISCOUNTED = 2;

	public static double getCharge(int daysRented) {
		double result = 0;
		result = result + BASE_PRICE;
		if (daysRented > DAYS_DISCOUNTED) {
			result = result + (daysRented - DAYS_DISCOUNTED) * PRICE_PER_DAY;
		}
		return result;
	}
}

// Die Klasse Customer wird angepasst:
public class Customer {
	private double totalCharge = 0;
	public void rentMovie(int daysRented) {
	totalCharge = totalCharge + Movie.getCharge(daysRented);
	}
	public double getTotalCharge() {
		return totalCharge;
	}
}
...

#### Abschliessende Reflexion
Am Ende auf die Programmierepisode zurückblicken und die Arbeit reflektieren. Ist der Code klar und Verständlich? Fehlen Tests? Was könnte in Zukunft besser gemacht werden.

* Customer ist nur noch für Summierung zuständig. Tests minimalisiert.
 ''' Java
 class CustomerTest {
	private Customer customer;
	
	@BeforeEach
	public void setUp() {
	customer = new Customer();  //Instanz mit Customerkonstruktor erstellen
	}
	
	@Test
	void testRentingNoMovie() {
		assertEquals(0, customer.getTotalCharge(), 0.001);
		// 0.001 ist die Toleranz bei Fliesskommazahlen
	}

	@Test
	void testRentingOneMovie() {
		customer.rentMovie(1); // Ausleihe ein Tag
		assertEquals(2, customer.getTotalCharge(), 0.001);
	}


	@Test
	void testRentingThreeMovies()  {
		customer.rentMovie(2); // Ausleihe zwei Tage
		customer.rentMovie(3); // Asuleihe drei Tage
		customer.rentMovie(4); // Ausleihe vier Tage
		assertEquals(11.25, customer.getTotalCharge(), 0.001);
	}
}

#### Integration, Abgleich eigener Code mit Source
Am Ende sollte der Code hochgeladen werden wenn mehrere Leute damit arbeiten. Und die Tests für den gesamten Code durchgeführt werden (Wenn dies nur ein Teil eines Projekts war). Und die aktuellen Veränderungen mit einem Kommentar einchecken.

## Kap 3 Unit Tests mit JUnit
### Unit Tests Definition
* Unit Test = kleinere Programmteile in Isolation von anderen Teilen
* Unit Test = einzelne Methoden, Klassen bis Komponenten
* Unit Test = White-Box-Test, kennt die Impletmentiereungsdetails

### JUnit Geschichte
Kern von Kent Beck und Erich Gamma.

### Frameworks für andere Sprachen?
?

### Installation
Da bereits ein eigene aktuelle Beschreibung dazu besteht, siehe dazu JUnit.md.

### Erstes Beispiel - Euroklasse
* Genügt nicht der Finanzwirtschaft aber für KMU ok. 
* Referenzobjekt vs. Wertobjekt, Wertobjekt wird bei jeder Interaktion neu erstellt, Bsp. add 2.- Euro = new, kein Setzen der Attribute Wert! Wei bei String. (Im Code bedeudet dies, der Konstruktor ist private, wird im Buch unter TestCase kurz beschrieben.)

### Klasse testen - bietet Möglichkeiten
Test schreiben = Gelegenheit über öffentliche Schnittstelle nachzudenken.
Was erwarten wir von der Klasse?

### JUnit Source Code
Wie kann dieser in Eclipse nachträglich installiert werden? Antwort evlt. verschieben zu Eclipse oder Junit.md.

### Assertions
Werden hier nicht nochmal behandelt, siehe JUnit.md.

### AssertionFailedError
Bei einem Fehler wird abgebrochen und auf die Test Methode inkl. Zeile verwiesen. Mehr sonst unter Kap 3.8 im PDF-Buch.

Empfehlung im Code gleich eine Fehlermeldung mitgeben:
''' Java
assertEquals("amount not rounded", 2.00, rounded.getAmount(), 0.001); // Fehlermeldung innerhalb der ""
'''

### TestCase, der Testfall
* 

