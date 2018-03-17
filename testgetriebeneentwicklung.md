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

### TestCase, der Testfall, setUp() & tearDown()
* Gedanken immer sofort notieren, Bsp. als // TODO Kommentar in der Testklasse
* Übergang von Einzelnen Tests zu Test Gruppen -> gleiche Inhalte mit @BeforeEach zusammenfassen
* Organisiere Testklassen um gemeinsame Testfälle, nicht um die getestete Klasse

Zusammenfassung einzelner Testfälle
Testfälle welche die gleichen Voraussetzungen erfordern bzw. die gleichen Objekte instanzieren als Test setUp() aufsetzen. Wobei die Instanzvariable vor dem setUp() steht, die Initialisierung in der setUp() Methode geschiet. In der tearDown() Methode werden vor allem Netz- und Datenbankverbindungen freigegeben.
''' Java
public class EuroTest...
	private Euro two;
	protected void setUp() {
		two = new Euro(2.00);
	}
...

### Testablauf verstehen
Wie läufen die Tests von JUnit ab? Alle mit @Test bezeichneten Methoden bezeichnen ein Testobjekt. Ablauf Testobjekt setUp(), testMethode(), tearDown(), nächstes Objekt. Somit besteht zwischen den Testobjekten keine Verbindung!
Anders gesagt aus der Testfallklasse entsteht pro Testmethode (@Test) ein Testobjekt:Testklasse bzw. anonyme Testobjekte.
'''
                    --------------
                    | :EuroTest  |                         --------------
                    --------------                         | :EuroTest  | 
            setUp()  -->  |                                --------------  
@Test testMethode1() -->  |                           setUp() -->  |   
         tearDown()  -->  |              @Test testMethode2() -->  | 
                          X                        tearDown() -->  |
                                                                   X
'''

### TestSuite
Zusammenfassung mehrer TestCase in eine Suite. Ziel neuer Code auf alle Zusammenhänge hin testen. Möglichst in kurzen Abständen.
* Als Zusammefassende Klasse für TestCase kann eine Klasse 'AllTests' erstellt werden.
* In Eclipse mit dem erstell 'Wizzard' -> neue TestSuite erstellen!
''' Java
public class AllTests {
 public static Test suite() { // Methode zum Zusammenfassen
 TestSuite suite = new TestSuite();
 suite.addTestSuite(CustomerTest.class); // addTestSuite zum Hinzufügen von Klassen
 suite.addTestSuite(EuroTest.class);
 suite.addTestSuite(MovieTest.class);
 return suite;
 }
 '''
          -----------------    *
          | <<interface>> |   <---------------
          |     Test      |                  |
          -----------------                  |
                 / \                         |
<<impelements>>   |                          |
                  |                          |
             -------------------             |
             |                 |             |
       -------------    --------------       |		  
       |  TestCase |    | Testsuite  | <>--- |
       -------------    --------------  
'''
* Test abstrahiert von Testfällen und Testsuiten.
* TestSuite führt eine Reihe von Tests zusammen aus.

Umgang mit Paketen? ***Eins zu eins Kopei aus dem Buch, Bedeutung unklar***
Momentan liegen alle Klassen einfach im Default-Package. In der
Regel verteilen wir unsere Klassen natürlich über verschiedene Pakete
und müssen das Package beim Bündeln der Testsuite mit spezifizieren.
In den meisten Fällen ist es ohnehin praktisch, pro Package eine
Testsuiteklasse zu definieren. Nach diesem Muster können Sie dann
auch Hierarchien von Hierarchien von Testsuiten bilden:
suite.addTest(other.AllTests.suite());

### Testen ohne IDE (Eclipse)
Es ist möglich JUnit nur via CLI auszuführen. Details auf der Webseite JUnit.org. Auch eine GUI-Variante existiert ohne IDE.

### Testen von Exeptions
Gemäss Westphal ist zu Unterscheiden vorhersehbare Exceptions und unvorhersehbare. Gemäss JUnit5 Handbuch gibt es nur einen winzigen Eintrag zu Exception Handling welche ich nicht verstehe. Ergo dieses Thema muss noch vertieft werden.
Auszug Westphal JUnit3:
''' java
// vorhersehbare Exceptions
public class EuroTest...
 public void testNegativeAmount() {
 try {
 final double NEGATIVE_AMOUNT = -2.00;
 new Euro(NEGATIVE_AMOUNT);
 fail("amount must not be negative");
 } catch (IllegalArgumentException expected) {
 }
 }
}

public class Euro...
 public Euro(double euro) {
 if (euro < 0)
 throw new IllegalArgumentException("negative amount");
 cents = Math.round(euro * 100.0);
 }
}
'''

## Kap 4 Testgetriebene Programmierung
Testgetrieben Entwicklung ist:
* testgetriebene Programmierung
* Refactoring
* häufige Integration (Code zusammenführen)

1. testgetriebene Entwicklung
* Bevor wir neuen Code schreiben, schreiben wir neue Tests. 
* Bevor wir bestehenden Code ändern, ändern wir bestehende Tests. 
* Bevor wir einen Fehler im Code suchen und reparieren, suchen und reparieren wir das dafür verantwortliche Loch in den Tests.

Zyklus
* grün -> rot; neue Tests werden erstellt. 
* rot -> grün; Test wird im Code erfüllt.
* grün -> grün; Code vereinfachen. 

### Testepisode 4.4
Es geht darum neu erstellte Test Klassen nicht zu vergessen. Wird bestimmt durch die Vorgehensweise. Mit JUnit5 sollte dies kein Problem sein wenn ein Test besteht welcher allte Testklassen erkennt und auswertet. Zudem sollten Tests mit dem Tag @DisplayName(value = "TagName") gekennzeichnet werden. Dieser Tag kann dann beim Testlauf gesucht werden. Zum überprüfen das die neuen Tests auch ausgewertet werden.
Bsp. RegularPrice für JUnit4: Tip auch neue Klassen im Prinzip grün -> rot einfügen, d.h. nach erstellen soll die Klasse zB. in der Testsuite() als Fehler gemeldet werden weil zwar in der suite hinzugefügt aber noch keine testmethode definiert. 

### Testplan - Analyse
Bsp. neue Preise - zwei Faktoren:
1. Filme kosten neu 1.50 Euro für die ersten 3 Tage.
2. für jeden weiteren Tag 1.50 Euro zusätzlich.

Welche Werte sind also zu testen, bzw. welche Ausleihtage müssen überprüft werden?
Es macht Sinn die Methode abzufragen für die Tage 1, 3, 4, 5. Aber der Tag zwei z.Bsp. macht keinen Sinn. Grund Grenzwerte testen.

### Test fehlschlagen sehen, weshalb?
Jeder kleine Schritt testen. Das schlimmste ist wenn der Code nicht das macht was man denkt. Wenn eine Klasse schon besteht und eine neue an einem anderen Ort erstellt wird wäre dies sehr schlecht. Eclipse hilft hier deshalb wenn mit Eclipse ist dies nicht ein sehr grosses Problem aber ohne Eclipse müsste wirklich jeder Schritt einzeln gestestet werden.
Auch die Rückgabe 'Return Null' testen obwohl die Rückgabe Euro erwartet wird. Neuer Code wird in dieser Phase nur geschrieben wenn der Test fehlgeschlagen hat.
**Zudem soll der Test beim ersten mal auch Fehlschlagen, damit klar ersichtlich ist das der Test auch etwas Wert hat. D.h. der Test nicht von alleine auf grün stellt!**

### Test erfüllen
#### Nur soviel Code schreiben das alle Tests laufen
**Wiederholung:** Beim Schreiben des Codes z.B. return Wert nicht definieren und Testen. Obwohl die Erwartung klar ist das der Test fehlschlagen sollte muss dies getest werden!! Also 'Return Null' auch testen. Siehe auch vorderes Kaptiel. Bevor der Test dann erfüllt wird.
Immer den einfachsten Weg nehmen, auch wenn klar ist das dieser evtl. später nicht mehr ausreicht. Es geht nur darum den aktuellen Test zu erfüllen.
Ist der Test durch den Code erfüllt wissen wir nun dieser Teil des Codes stimmt. Erfolg. Bleibt er rot wissen wir auch dieser Teil stimmt noch nicht.

### Zusammenspiel von Test- und Programmcode
Schreibe immer nur einen Testfall und dann den Code dazu. Um nötige Testfälle zu notieren und nicht zu vergessen benutze ein System, z.B. TODO Liste oder Notizzettel.
* Ein fehlgeschlagener Test weisst an welchen Code als nächstes geschrieben werden muss.
* Aus dem bestehenden Code wird der nächste Test hergeleitet und treibt das Design voran.

Begründung weshalb kein Code ohne Test geschrieben werden sollte: Wenn der Code mehr kann als der Testfall besteht die Gefahr das Teile davon in keinem Test gesichert sind. Deshalb wird das Sicherheitsnetz der Tests beim Refactoring kleiner und Fehler können eher unbemerkt bleiben.

### Ausnahmebehandlung, Exception
1. Schreiben Sie einen Test, der zunächst fehlschlagen sollte.
2. Schreiben Sie gerade so viel Code, dass der Test fehlschlägt.
3. Schreiben Sie gerade so viel Code, dass alle Tests laufen.

Ausnahmen:
2a. Unerwartet Erfolg: grün -> grün: Der Test sollte fehlschlagen. Warum läuft er nur?
3a. rot -> rot: Der Test sollte laufen. Warum schlägt er jetzt fehl?
3b. rot -> ???: Der Test sollte sich in relativ kurzer Zeit mit relativ
wenig Aufwand erfüllen lassen. Wie kommen wir dort hin?

### 4.11 Unerwarteter Erfolg
Eigentlich sollte der intelligente Test den Code auf unterschiedliche Gegebenheite prüfen. D.h. der nächste Test sollte den nächsten programmier Schritt einleiten. Manchmal prüft läuft der 2., 3. Test schon auf anhieb. Wieso? Es gibt nur zwei Möglichkeiten:
* Der Code erfüllt die Anforderungen bereits
* Der Test enthält einen Fehler
#### Test enthält den Fehler
Prüfe den Test, allenfalls mit einem bekannten Fehler einbau zum Checken ob noch alles wie es sein sollte.
#### Code erfüllt Anforderungen
Liegt es nicht am Test selbst, sagt dies etwas über unseren Testprozess aus. Reflektiere:
* mehr Code geschrieben, als unbedingt notwendig wäre? (Beim letzten Test)
* Dieser Testfall schon anderswo mitgetestet? Vielleicht nur indirekt? Wie stark unterscheiden sich die Tests? Können wir sie geeignet zusammenfassen?
* Duplizieren wir gerade Testcode und wissen es vielleicht nicht?
* Haben die Tests zu wenig »Kick« dafür, dass sie interessante neue Entwurfsentscheidungen aufwerfen? Könnten wir solche Tests zukünftig ans Ende der Episode stellen? Ist es dann überhaupt noch notwendig, sie zu schreiben?
In unserem Fall Videoverleih haben wir quasi die Falsche Route gewählt. Es wäre besser gewesen erst 4 und 5 Tage zu testen und dann noch 3.
* Merke: Falls das schreiben der Test mehr mühe bereitet als schreiben des Codes, Tests vereinfachen, Testdesign überprüfen.

### 4.12 Unerwarteter Fehlschlag
#### 3a. Fehlschlag, sollte eigentlich laufen
Hier wird ein Beispiel genommen welches davon ausgeht das wir entweder den Code nicht in der einfachsten Form belassen, da wir wissen das die komplexität nicht der realität entspricht. Ausleihe 4 Tage = Preis Standard + Preis zusatz Tag, wenn wir in der Methode nur gib Preis 3 bei mehr als drei Tagen zurückgeben fehlt etwas an komplexität. Eigentlich ist doch das ein Widerspruch zum vorherigen Statment nur den Code zu schreiben der funktioniert, nicht mehr..
Jedenfalls, dadurch wird dann die Methode add aus der klasse Euro so verwendet das das Resultat den Test fehlschlägen lässt.
''' Java
public class RegularPrice {

	public Euro getCharge(int daysRented) {
//		new Code fails from book
		//		Euro result = new Euro(1.50);
//		if (daysRented == 4) result.add(new Euro(1.50));
//		return result;
		
////		this is not considered good, cause it does not represent the complexity
////		of having a base price and an increment
////		so in the example it gets changed with code further below
//		if (daysRented <= 3) return new Euro(1.50);
//		return new Euro(3.00);
		
		if (daysRented <= 3) return new Euro(1.50);
		return new Euro(1.50).add(new Euro(1.50));
	}

}
'''
Nebenbei ist der neue if Abschnitt verdächtig weil drei mal new Euro(1.50) vorkommt. Folgende Gedanken dazu:
* Wieso 3x?
* Was bedeuten die? Funktion ist nicht klar ersichtlich..
* Zusammenfassn möglich? Sind es separate Elemente oder lassen sie sich zusammenfassen?

Aber zuerst: Wieso geht nun die add-Operation im Vergleich zur result.add? Dies kommt daher wie die add Methode aufgbaut ist. Siehe dazu EuroTest:
''' Java
	@Test
	@DisplayName("testAdding, test the add Method")
	void testsAdding() {
		Euro sum = two.add(two);
		assertEquals(4.00, sum.getAmount(), 0.001);
		assertEquals(2.00, two.getAmount(), 0.001);
	}
'''
Vermutlich hat der Name add uns in die Irre geführt. Deshalb wir der zum Refactoring Kandidat. Notiz in aktuellem Test Case -> TODO refactoring rename Euro.add to Euro.plus
* Führe ein Refactoring durch, wenn du etwas gelernt hast. Und dieses Wissen Bestandteil des Programms sein sollte.

Beheben der drei new Euro(1.50) in drei Schritten:
* Umwandeln von drei Elementen in Konstanten, d.h. Konstanten wo möglich benennen!
* nach jedem Schritt Test laufen lassen
''' Java
public class RegularPrice...
 private static final Euro PRICE_PER_DAY = new Euro(1.50);
 public Euro getCharge(int daysRented) {
 if (daysRented <= 3) return BASEPRICE;
 return BASEPRICE.add(PRICE_PER_DAY);
 }
}

public class RegularPrice...
 private static final Euro PRICE_PER_DAY = new Euro(1.50);
 public Euro getCharge(int daysRented) {
 if (daysRented <= 3) return BASEPRICE;
 return BASEPRICE.add(PRICE_PER_DAY);
 }
}

public class RegularPrice... 
 private static final int DAYS_DISCOUNTED = 3;
 public Euro getCharge(int daysRented) {
 if (daysRented <= DAYS_DISCOUNTED) return BASEPRICE;
 return BASEPRICE.add(PRICE_PER_DAY);
 }
}
'''

Rückschritt für den Fortschritt
Bei Problemen wie weit zurück gehen? Dies hängt von der Schwierigkeit des Problems ab.
* Als erstes meist zurück zum Code welcher vor dem neuen Test noch gültig war, sprich einfach den neuen Code verwerfen und neu beginnen
* Falls das erfüllen des Tests zu viel Code fordert und allenfalls nicht mehr weiterkommen, auch den Test rückgängig machen und vom grünen Balken an neu beginnen.
Rückschritte können durch IDE einfach bewerkstelligt werden mit der Historie und können sogar wiederhergestellt werden. Bsp. Eclipse > compare with > local history

### 4.13 Vorprogrammierte Schwierigkeiten
3b. Test erfordert neue Methoden und damit weitere Tests
Wenn der Code kompliziert wird, heisst dies meist etwas mit unserem Code stimmt nicht! Im Beispiel fehlt der Klasse Euro, die Funktion Geldbeträge zu vervielfachen.
''' Java
public class RegularPrice...
 public Euro getCharge(int daysRented) {
 if (daysRented <= DAYS_DISCOUNTED) return BASEPRICE;
 int additionalDays = daysRented - DAYS_DISCOUNTED;
 return BASEPRICE.add(new Euro(PRICE_PER_DAY.getAmount()
 * additionalDays)); // Hint to problem =)
 }
}
'''
Trotzdem muss dieser hässliche Weg gewählt werden und der Test erfüllt werden!
Nur ein grüner Balken ist eine seriöse Grundlage fürs Refactoring. **Achtung:** Hier ist Disziplin gefragt. Obwohl es scheint ja nur schnell die Klasse anzupassen muss dies auf später verschoben werden. Nie den richtigen Weg verlassen!
**Refactoring nur mit einem grünen Balken.** 
* Nie an mehreren Enden gleichzeitig arbeiten.
* Probleme immer nacheinander lösen.
* Probleme in kleinere aufbrechen und jeden Schritt testen.

Bsp. neue Methode einfügen
* Test in entsprechender Testklasse schreiben
* Test laufen lassen
* Methode gemäss Test intialisieren
* Test laufen lassen
* Methode Funktion schreiben
* Test laufen lassen, jetzt sollte er grün sein

''' Java
// from EuroTest Class
	...
	@Test
	@DisplayName("test multiply")
	void testMultiplying() {
		Euro result = two.times(7);
		assertEquals(14, result.getAmount());
		assertEquals(2, two.getAmount());
	}
	...
// from Euro Class
	...
	public Euro times(int factor) {
		return new Euro(cents * factor);
	}
	...
'''


