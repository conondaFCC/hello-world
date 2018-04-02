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

### 1. Direktive - Jede Änderung via automatisierten Test
Motiviere jede Änderung des Programmverhaltens durch einen automatisierten Test.

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
**Wiederholung:** Beim Schreiben des Codes z.B. return Wert nicht definieren und Testen. Obwohl die Erwartung klar ist das der Test fehlschlagen sollte muss dies getest werden!! Also 'Return Null' auch testen. Siehe auch vorderes Kapitel. Bevor der Test dann erfüllt wird.
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
#### von neuen Methoden während dem Testen
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

Nun lässt sich der Code für getCharge mit der neuen times Methode vereinfachen:
''' Java
public class RegularPrice {
...
	public Euro getCharge(int daysRented) {
		if (daysRented <= DAYS_DISCOUNTED) return BASERPICE;
		int additionalDays = daysRented - DAYS_DISCOUNTED;
		return BASERPICE.add(PRICE_PER_DAY.times(additionalDays));
	}
}
'''

Weshalb die Methode times nicht direkt mitenwickelt wurde? Da wir sonst für die times Methode nicht einen eigenen Test hätten schreiben können im Sinne nur ein aktiver Testfall.

#### offene Tests ein Zeichen für Murks
Bleibt ein Test oder soll ein zweiter eröffnet werden, d.h. der erste würde offne bleiben über einen längeren Zeitraum ist dies ein Zeichen das die Zerlegun der Aufgabe in Testfälle schlecht ungeschickt.

Nun kann es die Situation geben wo wir meinen es gäbe keinen einfacheren Weg als direkt eine neue Klasse / Methode zu schreiben. Obwohl der Autor Westphal meint es gäbe immer die möglichkeit wie obenbeschrieben im aktuellen Test die Mehtode erst umständlich auszuformuieren bis der Test läuft! Trotzdem soll hier zu diesem Ansatz etwas stehen: gem. S. 71 im Buch. Vorsicht beim Top-down design und der Bottom-up implementierung insofern, dass es gefährlich wird sich am Ziel vorbeizuarbeiten.

#### Zwei offene Tests, einer zu viel
Grund der einzelne Test bekommt seinen Wert dadurch das er alleine also nur für eine Sache zuständig ist. Bei zwei roten Balken geht diese Vorteil verloren. Code wird nicht mehr minimal geschrieben.

### 4.14 Kleine Schritte gehen
Heisst dies alles muss in winzigen Schritten getestet werden? Nein, es heisst das dies ein gutes Beispiel war wie klein die Schrite sein können. Das Tempo bzw. die grösse der Schritte kann ich selber wählen anhand meinem Gefühl. Um sein eigenes Tempo zu finden braucht es Erfahrung, Erfahrung heisst auch sich zu überschätzen und dabei zu lernen.

#### Füsse trocken halten
Ziel Fluss mit trockenen Schuhen überqueren, also von Stein zu Stein gehen oder weiter entfernte Steine springen. Am Ende die Schuhe vergleichen. =)

## Refactoring
Was heisst Refactoring?
Ziel ist Code verbessern, d.h. verständlicher und einfacher änderbar machen, ohne das Verahlten zu verändern. Refactoring ist eine Investition in die Zukunft des Codes.
In einem Satz: Allzeit sauberer Code.

### 5.1 Die zweite Direktive
Die zweite Direktive besagt:
**Bringe deinen Code immer in die Einfache Form.**
Diese Regel sagt und, wann Refactoring ins Spiel kommt. Wir wollen sauberen Code, der funktioniert.
Weshalb? Sonst wird Code schwer lesbar und damit zeitintensiv.

Zwei Ansätze wie Refactoring Code schreiben vereinfacht:
* Zum erfüllen des Test, bringen wir Code vorher in Form. (Optional, Kürteil)
* Nach dem der Test läuft, wird refaktorisiert und der Code in die Einfache Form gebracht. (Pflichtteil)

### 5.2 Die Refactoringzüge
Refactoring in Kürze:
1. Spüre schlecht riechenden Code auf.
2. Plane Refactoringroute, um Design zu verbessern.
3. Refaktorisiere in kleinen Schritten und teste oft.

Notizen: Wenn eine Möglichkeit zur Vereinfachung auffält notieren bevor sie verloren geht. Augen offen halten für verbesserungswürdigen Code.

Refaktorisiere erst wenn die Erkenntnis für neue Desing klar ist. Ansonsten nur die Notiz machen.

Code als etwas Organisches begreifen, knetbar und formbar. Hinweise: "Ich bin hässlich!", "Ich werde zu fett!", "Niemand versteht mich!". Code will strukturiert werden.

### 5.3 Von übel riechendem Code
#### Schrit 1: Aufspüren
Halte dich an die Designprinzipien und Entwurfsmuster welche hier vorgestellt werden. Schlechter Code finden mit eigener Intuition.

Kent Beck und Marin Fowler beschreiben im Refactoringbuch [fo99] über zwanzig 'Code Smells' welche beste Ansätze liefern:
* **duplizierte Logik** - Nummer 1. der Gestankliga und oft Problemverursacher. Kopien, bewusst oder unbewusst beginnen ein eigenleben.
* **grosse Klassen und lange Methoden** - verschlechtern Mobilität, Lokalisation, Verwendung und Dokumentation und sind Nährboden für Duplikation.
* **schlechte Namen und unpassende Codestrukturen** - erhöhen Lernkurve
* **spekulative Verallgemeinerungen** - Designelemente die nur im Weg stehen
* **Kommentare** - oft das Deodorant von schlecht riechendem Code. Nach gutem Refactoring überflüssig. Nicht entbehrlich, Entwurfsentscheidungen, veröffentliche Schnittstellen in Javadoc oder Literaturhinweise.

### 5.4 über den Refactoringkatalog
#### Schritt 2. Plane die Refactoringroute um das Design zu verbessern
Design ist etwas Organisches. Entscheidungen von gestern können heute veraltet sein.

Merke: Jede Entwurfsentscheidung ist umkehrbar.
Software ist änderbar. Ohne die Semantik, das Verhalten, zu verändern, kann ich:
* Klassen teilen oder zusammenführen,
* Methoden und Attribute in andere Klassen verschieben,
* Klassen, Methoden, Paramter und Variablen umbenennen,
* Methoden extrahieren oder integrieren, (verschieben oder an Ort implemtieren)
* Vererbung durch Delegatin ersetzen oder umgekehrt sowie
* Schnittstellen einziehen.

Refactoringroute wählen, wenn die Sachen grösser wird, Restrukturierung ist auch nur Refaktorisierung in kleinen Schritten, bewegen wir uns auf die Welt der Design Patterns zu, da dort die besten Lösunge für bekannte Probleme sind.

Ein Ansatz Lösungen zu finden wäre Martin Fowler's Buch welches auch 70 Refactoring detailiert beschreibt und zeigt welche Risiken bestehen.

Auch der Onlinekatalog mag einen Blick Wert sein: www.refactoring.com

#### Was-wäre-wenn-Betrachtung
Hilfreich ist die Was-wäre-wenn-Betrachtung um Entwurfsalternativen in Erwägung zu ziehen. Stell dir vor wie ein Refactoring das Design beinflusst.

### 5.5 zur Einfachen Form
#### Schritt 3: Refaktorisiere oft in kleinen Schritten und teste
Design hat die einfache Form, wenn der Code:
1. alle Tests efüllt. Nur Code mit guten Testfällen kann mit Vertrauen refaktorisiert werden. **Achtung!** Missglücktes Refactoring kann Stunden vernichten. Evlt. erst bremsen und mehr Tests schreiben.
2. seine Intention klar ausdrückt. Es ist meine Aufgabe alle Ideen durch den Code zu kommunizieren. Code wird öfter gelesen und geändert als geschrieben! Deshalb sind verständliche Namen für kleinste Codeteile wichtig. Drücke in den Namen immer nur die Intention aus, nicht die Implementierung selbst. Dadurch lässt sich Code schreiben der sich selbst dokumentiert.
3. keine duplizierte Logik enthält. Schreibe nie die gleiche Zeile Code zweimal! Selbst kleine Codeduplikate riechen übel! Duplikationen sind Lernerlebnise, eine neue Abstraktion wartet darauf entdeckt und belebt zu werden.
4. möglichst wenig Klassen und Methoden umfasst. Keine Schweizertaschenmesser programmieren. Sondern viele kleine Klasssen mit vielen kurzen Methoden. Der einzige Weg zu weniger Klassen und Methoden zu kommen ist unnötige Designelemente entfernen: Es gibt eine Grauzone, wo eine grössere Methode die Absicht besser vermittelt als zwei kleinere Methoden oder wo eine Abstraktion die Intention verbirgt und Duplikation ok ist.

### 5.6 Überlegungen zur Refactoringroute
Refactoring sollte sich wie ein Lauffeuer über weite Systemteile verbreiten. Im Idealfall wird eine Änderung an einer Klasse sich auf den abhängigen Code übertragen. Refactoringziel muss durch eine Folge kleiner Schritte erreichbar sein. Sonst geht Code in Flammen auf. D.h. Disziplin und Voraussicht. Strategie Refactoring halbieren:
* **Implementierungssubstitution** berührt die Implementierungsdetails einer Klasse. Tests und Verwender bleiben unbetroffen.
* **Schnittstellenevolution** berührt primär öffentliche Schnittstelle. Tests und Verwender sind betroffen.

#### Beispiele aus bisherigen Beispielen
##### Euro:add in plus umbennen
Keine grosse Erklärung, mit guten IDE's einfach, wähle 'rename in Workspace', von Hand Tricky da additionalDays nicht ersetzt werden sollte mit plus.

Die nächsten drei Beispiele werden in den nächsten Kapiteln 5.7 und folgende behandelt.

### 5.7 Substitution einer Implementierung
Was in Kaptile 5.7  5.8 behandelt wird ist unter https://refactoring.guru/replace-data-value-with-object einfach zum Nachschlagen beschrieben und enthält auch mehr Infos zum Hintergrund wieso dieser Refactoring Schritt gemacht werden sollte. Nebenbei die Seite refactoring.guru entspricht vom Inhalt her ziemlich genau dem Buch von Herr Fowler! http://my.safaribooksonline.com/0-201-485672/ch08lev1sec2

#### Customer und Movie auf Euro umstellen
Code Smell: - primitive Obsession, aus dem Buch von Herr Fowler.
Mehr dazu auch unter: https://espeo.eu/blog/code-quality-primitive-obsession-code-smells/
Anstatt primitive Datentypen zu verwenden, verwenden wir Objekte.
''' Java
//Alter Code:
public class Movie {
 private static final double BASE_PRICE = 2.00; // Euro
 private static final double PRICE_PER_DAY = 1.75; // Euro
 private static final int DAYS_DISCOUNTED = 2;
 public static double getCharge(int daysRented) {
  double result = BASE_PRICE;
  if (daysRented > DAYS_DISCOUNTED) {
  result += (daysRented - DAYS_DISCOUNTED) * PRICE_PER_DAY;
 }
 return result;
 }
 // Neuer Code:
 public class Movie {
 private static final Euro BASE_PRICE = new Euro(2.00);
 private static final Euro PRICE_PER_DAY = new Euro(1.75);
 private static final int DAYS_DISCOUNTED = 2;
 public static double getCharge(int daysRented) {
  Euro result = BASE_PRICE;
  if (daysRented > DAYS_DISCOUNTED) {
  int additionalDays = daysRented - DAYS_DISCOUNTED;
  result = result.plus(PRICE_PER_DAY.times(additionalDays));
  }
 return result.getAmount();
 }
}
'''
Dabei konnte der Kommentar 'Euro' entfernt werden und die Methode zur Berechnung der Kosten gemäss Bsp. RegularPrice angepasst. Nun haben wir doch aber ein Duplikat?

### 5.8 Evolution einer Schnittstelle
#### Schnittstellen und wichtige Gedanken dazu
* Eine Änderung an den Schnittstellen wirkt sich auf alle abhängigen Systemteile aus. Dies ist ein zwei schneidiges Schwert. Wir wollen diesen Effekt und wir fürchten ihn. Oft sind die Aufwände dazu hoch. Dadurch hat sich die Kultur entwickelt,Schnitstellen möglichst wenig zu ändern und abwärtskompatibel zu sein. Dies gilt aber nur wenn wir den Quellcode nicht haben oder die Schnittstellen veröffentlichen!
* In allen anderen Fällen sind auch Schnittstellen leicht veränderbar.

Für die Refactoringroutenplanung nehme ich mit ein Refactoringschritt sollte:
* Schnitstellen möglichst wenig ändern
* bei Änderungen abwärtskompatibel sein

#### Refactoringroute à la Tamo Freese via tmpMethode
Jeder Schritt bleibt via JUnit testbar!
Schritt eins: beachte Schritt 5.7 ist allenfalls vorher zu vollziehen!
Alte Methode extrahieren ohne return statement mit der Refactoring Methode von Eclipse. Bewirkt das Eclipse automatisch die Zeile hinzugefügt: 'Euro result = tmpCharge(daysRented);'
''' Java
	public static double getCharge(int daysRented) {
		Euro result = BASE_PRICE; // Extrahierter Teil von
		if (daysRented > DAYS_DISCOUNTED) {
			int additionalDays = daysRented - DAYS_DISCOUNTED;
			result = result.plus(PRICE_PER_DAY.times(additionalDays));
		} // bis HIER!!
		return result.getAmount();
	}
'''
Mit Exclipse > Refactor > extract Method -> neuer Name für Methode geben
''' Java
	public static double getCharge(int daysRented) {
		Euro result = tmpCharge(daysRented);
		return result.getAmount();
	}

	public static Euro tmpCharge(int daysRented) {
		Euro result = BASE_PRICE;
		if (daysRented > DAYS_DISCOUNTED) {
			int additionalDays = daysRented - DAYS_DISCOUNTED;
			result = result.plus(PRICE_PER_DAY.times(additionalDays));
		}
		return result;
	}
'''

Schritt zwei: Variable in getCharge ersetzen mit dem Ausdruck selbst! Adapterfunktion um abwärtskompatibel zu sein.
''' Java
public class Movie... 
 public static double getCharge(int daysRented) {
 return tmpCharge(daysRented).getAmount();
 }
}
'''
Schritt drei: alle alten Methodenaufrufe in Tests und anderen Klassen anpassen
Ein Weg dies zu machen, damit kein Aufruf vergessen geht ist mit der Eclipse rename in Workspace funktion auf der Methode selbst. Dann einfach die Methode wieder umbennen nachdem alle auf die tmp umgestellt wurden. Ziel die Methode 'getCharge' soll von nirgends mehr verwendet werden.
''' Java
public class MovieTest extends TestCase { JUnit: OK
 public void testBasePrice() {
 assertEquals(2.00, Movie.tmpCharge(1).getAmount(), 0.001);
 assertEquals(2.00, Movie.tmpCharge(2).getAmount(), 0.001);
 }
 public void testPricePerDay() {
 assertEquals(3.75, Movie.tmpCharge(3).getAmount(), 0.001);
 assertEquals(5.50, Movie.tmpCharge(4).getAmount(), 0.001);
 }
}
public class Customer...
 public void rentMovie(int daysRented) {
 totalCharge += Movie.tmpCharge(daysRented).getAmount();
 }
}
'''
Dank den Tests habe ich hier dann einen Fehler gefunden und dieser kann einfach behoben werden. Klasse CustomerTest verlangt 'assertEquals(2, customer.getTotalCharge(), 0.001);', also musste bei der Klasse Customer die Methode rentMovie neben der Anpassung mit tmpCharge auch noch mit .getAmount() ergänzt werden. Dank dem wird dies dann klar!

Schritt vier: Wenn alle Tests grün sind und alle Beziehungen angepasst sind, macht es Sinn die alte Methode in OLD umzubenennen. Damit ist klar das die Methode nicht mehr verwendet werden sollte. Test laufen lassen.

Schritt fünf: tmpCharge in getCharge umbenennen
Konkret wurde die Methode in Kap 5.7 so geändert das getCharge neu ein Euro Objekt zurückgibt anstatt eine Zahl, siehe Kap 5.7 und in 5.8 wird die Methode getCharge so refacotred, dass diese eben nicht den Betrag der Instanz Euro ausgibt sondern nur das Objekt übergibt. Der Test soll aber den Betrag prüfen und nicht das Objekt selber (Objekte vergleichen, kann ich ja nicht wenn es nicht die gleichen sind, Hashwert!), also die Methode getAmount verwenden. In einem Satz die Verwendung der getAmount Methode ausgelagert von der Klassenmethode getCharge direkt in die Testklasse. Alles klar? Zudem sollte klar sein das die Regel gilt: Um eine Klasse schöner zu machen, muss eine andere hässlicher werden! Jedenfalls im Schritt fünf.

Schritt sechs: Nun die Testklasse anpassen. 
''' Java
public class MovieTest...
 public void testBasePrice() {
 assertEquals(new Euro(2.00), Movie.getCharge(1));
 assertEquals(2.00, Movie.getCharge(2).getAmount(), 0.001);
 }
}
'''
Die geht mit JUnit 5 nicht mehr weil die Hash Werte von new Euro(2) und Movie.getCharge(1) nicht gleich sind und es somit nicht die gleichen Objekte sind. Darauf hin habe ich im Internet nach einer Lösung für dieses Problem gesucht. Die Antwort scheint zu sein. Es macht eigentlich keinen Sinn in einem Unit Test zwei Objekte zu vergleichen. Es sollten nur die Funktionen getestet werden und somit nur Werte oder Paramter von Objekten verglichen werden. Siehe: https://softwareengineering.stackexchange.com/questions/300345/ways-of-creating-expected-object-for-assert

Deshalb scheint der einzige Weg die Test mit konform mit der neuen Methode zu schreiben kompliziert zu sein und den Test Code nicht zu vereinfachen.
''' Java
assertEquals(new Euro(2.00).getAmount(), Movie.getCharge(1).getAmount());
'''

Im Buch geht es weiter mit Customer auf Euro umstellen gem. 5.7 und 5.8 CustomerTest auch.
Beim Anpassen von Customer auf Euro als Klassenvariablen, Problem: zwei Euro Klassen zusammenzählen. Lösung wurde schon vorher erstellt, eine Methode welche zwei Euro Klassen zusammenzählen kann. Euro.plus(otherEuro).
Klasse Customer und CustomerTest umgestellt. Wie bei Movie mit dem vorbehalt das Vergleich von Objekten nicht möglich ist und deshalb beide Euro Objekte dessen Werte verglichen werden mit getAmount.

### 5.9 Teilen von Klassen
Wieso? Gründe kann es mehrere geben. Einer kann sein das der Code zu kompliziert ist und durch das aufteilen einer Klasse klarer wird. Kann auch den nächsten Schritt sichtbar machen, welcher sonst evtl. gar nicht sichtbar wäre.

Siehe auch: https://refactoring.guru/extract-class

Merke das Aufteilen der Klasse ist nur ein Schritt, weitere werden in den folgenden Unterkapiteln 5.10 usw. erklärt.

Im Bsp. dupliziert die Klasse Movie die Regeln zu Berechnung der Ausleihgebühr. Grund ist das die Klasse eine andere Klasse in sich trägt.
Neue Klasse erstellen, Schritt 1, Methode extrahieren:
* in Eclipse, Methoden Inhalt selektieren, und via Refactor > Extract Method in neue Methode verschieben (tmpMethode)
* die tmpMethode mit den Klassenvariablen in neue Klasse kopieren, wenn die Attribute nicht bestimmte Bezeichner besitzen lassen sich diese via Refactor > extract Class auswählen in Eclipse
* der alten Methode return Wert an neue Klasse anpassen (Klasse.tmpMethode mitgeben)

#### Methode extrahieren
''' Java
public class Movie...
 public static Euro getCharge(int daysRented) {
 Euro result = BASE_PRICE;
 if (daysRented > DAYS_DISCOUNTED) {
 int additionalDays = daysRented - DAYS_DISCOUNTED;
 result = result.plus(PRICE_PER_DAY.times(additionalDays));
 }
 return result;
 }
}
// Ergibt:
public class Movie... 
 public static Euro getCharge(int daysRented) {
 return tmpCharge(daysRented);
 }
 public static Euro tmpCharge(int daysRented) {
 Euro result = BASE_PRICE;
 if (daysRented > DAYS_DISCOUNTED) {
 int additionalDays = daysRented - DAYS_DISCOUNTED;
 result = result.plus(PRICE_PER_DAY.times(additionalDays));
 }
 return result;
 }
}
'''

JUnit Test machen.

#### Transfer Logik zu neuer Klasse
Methode und Variablen kopieren in die neu erstellte Klasse:
''' Java
public class NewReleasePrice {

	private static final Euro BASE_PRICE = new Euro(2.00); // Euro
	private static final Euro PRICE_PER_DAY = new Euro(1.75); // Euro
	private static final int DAYS_DISCOUNTED = 2;

	public static Euro tmpCharge(int daysRented) {
		Euro result = BASE_PRICE;
		if (daysRented > DAYS_DISCOUNTED) {
			int additionalDays = daysRented - DAYS_DISCOUNTED;
			result = result.plus(PRICE_PER_DAY.times(additionalDays));
		}
		return result;
	}
}
'''

Im Buch steht der Test sollte immer noch laufen, bei mir musste aber auf den Ort der Methode verwiesen werden *return **NewReleasePrice**.tmpCharge(daysRented);*.
''' Java
public class Movie {

	public static double getCharge_OLD(int daysRented) {
		return getCharge(daysRented).getAmount();
	}

	public static Euro getCharge(int daysRented) {
		return NewReleasePrice.tmpCharge(daysRented);
	}
}
'''

#### Methoden verweise bzw. Zugriffe anpassen
Nun die Methode wieder zurückbenennen:
* am besten mit Rename in Workspace, sollte dann auch die aktiv genutzten Methoden umbennen.
''' Java
public class NewReleasePrice...
 public static Euro getCharge(int daysRented)...
}
// Und auch:
public class Movie {
 public static Euro getCharge(int daysRented) {
 return NewReleasePrice.getCharge(daysRented);
 }
}
'''
Weshalb lassen wir diesen Code Teil in der Klasse Movie stehen? Es dient der Erklärung der Intention des Codes. Auch wenn dadruch es so aussieht als ob eine Klasse zu viel zwischen Code und Methode liegt.


### 5.10 Verschieben von Tests
Wandert Code von einer Klasse zur Anderen, wandern die Tests mit. Klassen ohne eigene Tests sind schlecht Klassen. Versuche nicht die Abkürzung mehrere Klassen, auch kleinere, in einer Klasse zu testen. Zudem wird oft eine neue Klasse erstellt gerade weil die Tests für die alte nur schwer zu formuliern waren. Oder die Klasse schlecht gestest war.

Fragen beim Code verschieben:
* Müssen relevante Tests nachziehen?
* Müssen weitere Test für die neue Klasse hinzu?
* Können zurückgebliebene Tests wegfallen?

Das Thema wird nur angeschnitten auf einer Seite. Die alten Test kopieren in neue Test Klasse und die Klassenbeziehungen anpassen. Am alten Testort ein alter Test stehen lassen und entsprechend beschriften.

### 5.11 Abstraktion statt Duplikation
#### Substitute Algorithm
Mehrfaches Vorkommen von gleichen oder änlicher Codeteile ist ein Indiz für Cade Vereinheitlichung, evtl. fehlt noch die passende Idee.
Vergleich der beiden Klassen NewReleasePrice und RegularPrice und Unterschiede herauslesen.
Test JUnit grün.

Schritt 1: Gleiche Teile vereinheitlichen.
Schritt 2: Superklasse extrahieren, child mit extends versehen
Schritt 3: Superklasse methode anpassen wenn nötig, umbenennen usw.
Schritt 4: Superklasse Konstruktor erstellen mit Parameter z.B.
Schritt 5: Subklasse Konstruktor verwenden mit Klassen Parameter.
Schritt 6: Auswirkungen auf andere Klassen anpassen. Verwendung von Klassen kann Anpassung beim Aufruf nötig machen.

#### Extract Superclass
Jede Idee sollte im Code nur einmal vorhanden sein. Befällt mich beim Code ein Déjà-vu-Erlebnis, finde den passenden Code und vereinheitliche bzw. führe zusammen. Dies führt zu vielen Klassen und einer Framework Struktur.

Wenn die Klassen vereinheitlicht sind, kann mit dem Eclipse Befehl 'Extract Superclass', die Superklasse automatisch extrahiert werden. Dabei müssen nur noch alle Variablen und Methoden ausgewählt werden. Bei welcher Klasse dies gemacht wird spielt keine Rolle.
''' Java
public class Price {
 private static final Euro BASEPRICE = new Euro(1.50);
 private static final Euro PRICE_PER_DAY = new Euro(1.50);
 private static final int DAYS_DISCOUNTED = 3;
 
 public Euro getCharge(int daysRented) {
 if (daysRented <= DAYS_DISCOUNTED) return BASEPRICE;
 int additionalDays = daysRented - DAYS_DISCOUNTED;
 return BASEPRICE.plus(PRICE_PER_DAY.times(additionalDays));
 }
}
public class RegularPrice extends Price {
}
'''
Test JUnit grün.

Methode in die Superklasse? Wenn die selbe Methode von allen Subklassen verwendet wird macht es Sinn die Methode in der Superklasse zu schreiben.

#### Variable Teile vs. fixe aka Template Method Pattern
https://refactoring.guru/design-patterns/template-method

Die static final Attribute ändern auf static und umbenennen in Klassenvariablen inkl. in den Methoden:
'private static Euro basePrice; private static Euro pricePerDay; private static int  daysDiscounted;'

Kostruktor für Subklassen erstellen
Der Konstruktor nimmt die Paramter der Subklasse und erstellt somit die Unterschiedlichen Berechnungen.
''' Java
public class Price...
 private Euro basePrice;
 private Euro pricePerDay;
 private int daysDiscounted;
 
 Price(Euro basePrice, Euro pricePerDay, int daysDiscounted) {
 this.basePrice = basePrice;
 this.pricePerDay = pricePerDay;
 this.daysDiscounted = daysDiscounted;
 }
 ...
}
'''

Subklassen auf Kontruktor umstellen
''' Java
public class RegularPrice extends Price {
 public RegularPrice() {
 super(new Euro(1.50), new Euro(1.50), 3);
 }
}

public class NewReleasePrice extends Price {
 public NewReleasePrice() {
 super(new Euro(2.00), new Euro(1.75), 2);
 }
}
'''

Abhängikeiten lösen:
// alt:
public class Movie { JUnit: OK
 public static Euro getCharge(int daysRented) {
 return NewReleasePrice.getCharge(daysRented);
 }
}
// neu:
public class Movie { JUnit: OK
 public static Euro getCharge(int daysRented) {
 return new NewReleasePrice().getCharge(daysRented);
 }
}
'''

### 5.12 Die letzte Durchsicht
Nach Refactoring alle Codeteile überprüfen. Kommunikation, keine Duplikation etc.

Letzte Episode:
* Lohnt sich Konzept RegualrPrice und NewReleasePrice als eigene Klassen?
* Tests als eigene Klassen oder Superklasse testen?

'''Java
public class Movie {
 public static Euro getCharge(int daysRented) {
 return Price.NEWRELEASE.getCharge(daysRented);
 }
}
JUnit: OK public class PriceTest extends TestCase {
 public void testBasePrice() {
 assertEquals(new Euro(2.00), Price.NEWRELEASE.getCharge(1));
 assertEquals(new Euro(2.00), Price.NEWRELEASE.getCharge(2));
 }
 public void testPricePerDay() {
 assertEquals(new Euro(3.75), Price.NEWRELEASE.getCharge(3));
 assertEquals(new Euro(5.50), Price.NEWRELEASE.getCharge(4));
 }
}
'''

Der Umsturz auf die neuen Tests. Wird mir zu wenig detailiert erklärt.
Die Klassen welche sich löschen lassen sind. RegularPrice und NewReleasePrice inkl. den zwei Testklassen davon.

Was ist mit dem Test der Price REGULAR? Wird gar nicht mehr getestet!

Probleme mit den Testergebnissen, nur wenn ich die Zeile Price REGLUAR Parameter auskommentiere laufen alle Tests. Diese Zeile überschreibt sonst alle Resultate. Fehler zwar gefunden aber erklären kann ich es nicht!
Erklärung: da die Klassenattribute alle auf static sind können diese nur einmal für alle Instanzen gelten.
Lösung: static bei Klassenattribute aufheben und bei den Methoden welche diese Attribute verwenden. Resultat: JUnit5 tests wieder grün.
''' Java
public class Price {

	private Euro basePrice;
	private Euro pricePerDay;
	private int  daysDiscounted;
	
	public final static Price NEWRELEASE = new Price(new Euro(2.00), new Euro(1.75), 2);
	public final static Price REGULAR = new Price(new Euro(1.50), new Euro(1.50), 3);

	public Price(Euro basePrice, Euro pricePerDay, int  daysDiscounted) {
		this.basePrice = basePrice;
		this.pricePerDay = pricePerDay;
		this.daysDiscounted = daysDiscounted;
	}

	public Euro getCharge(int daysRented) {
		if (daysRented <= daysDiscounted) return basePrice;

		int additionalDays = daysRented - daysDiscounted;
		return basePrice.plus(pricePerDay.times(additionalDays));
	}

	public Price() {
		super();
	}

	public Euro getBasePrice() {
		return basePrice;
	}
	
}
'''

### 5.13 Ist Design tot?
#### UML Design vs. evolution. Was ist besser?
Wenn alles klar ist macht UML schon sinn, sonst eben evolution und richtiges E
entdecken und lernen. Merke ohne Refactoring ist keine Rückkopplung zum UML, also fast keine Evolution möglich und damit die Beseitigung von 'code smells'.
Argument, Designphase gilt nur am Anfang der Entstehung, danach wird bei einem schnellen release-feedback loop, die weitere Entwicklung durch die Bedürfnisse gesteuert, was sich so nicht voraus designen lässt.
Siehe auch: Artikel »Is Design Dead?« [fo00] diskutiert Martin Fowler
sehr lesenswert das Thema geplantes kontra evolutionäres Design.

#### code-smell beheben aber wann?
Argument sofort beim Erkennen. Grund, sonst wird es nie gemacht. Ein verschieben auf später heisst eigentlich eine Ablehnung. zweiter Grund, oft ist ein solcher code-smell ein Hineweis, hier wurde etwas gemacht von jemandem der nicht alles versteht und deshalb noch weitere Sachen nicht versteht und somit Fehler sich eingeschlichen haben. Gemäss Studie SIGSOFT-Papier sogar bewiesen in Linux code. Argument kleine Probleme lösen erspart und die grösseren Probleme.
Die Lektion ist klar. Nichts ist zu klein, um behoben zu werden,
und jedem Geruch, egal wie schwach, muss nachgegangen werden.

konkretes vorgehen bei code smells:
* dem code smell nachgehen, weshalb wurde die Nase aktiviert? Abklären.
* Wenn möglich Problem sofort beheben.
* Sonst: Code smell beschreiben mit Kommentar und taggen, Bsp. eigenes Tag verwenden. Habe hier bereits eines welches heisst Code läuft aber sollte angepasst werden '// XXX'.
* Allenfalls: Zusicherung einfügen, wie Meldung bei Laufzeit, das dieser schadhafte Code ausgeführt wird, oder eine Meldung in die Logdatei schreibt, als Erinnerung Problem noch offen.
* **Folge Probleme Suchen**, nach Behebung des code smells, ist nur die halbe Miete. Zeit nehmen und andere Teile Quellcode durchsuchen die den Fehler enthielt. Ausschau halten was nicht koscher ist. Probleme beheben.
* Person welche Fehler gemacht hat, noch anderen Code verändert? Dort nach Fehlern suchen mit dem selben Fehlermuster.

Bsp. code smell Autor:
Serveranwendung Methode Liste von von Listen aktualisieren ohne 'synchronized' Modifizierer. Stellte sich heraus Entwickler wusste nicht wie in nebenläufigen Umgebungen zu programmieren. Viele Fehler gefunden bevor Produkt dem Kunden ausgeliefert wurde.

Fazit: der Nase vertrauen, ein kleiner Duft kann zu einem Misthaufen führen.
 Y. Xie and D. Engler: Using Redundancies to Find Errors. SIGSOFT
2002/FSE-10.

### 5.14 Richtungswechsel ... = neue Anforderungen an System, Einführung
Die nächsten Unterkapitel zeigen das Vorgehen bei der Weiterentwicklung des System mit neuen Vorgaben.

Neue Vorgaben:
* Rechnung erstellen
* Rechnung mit Information, welcher Kunde welchen Film wie lange ausgliehen hat
* Einzelposten, Gesamt Betrag
* Besonderes: Preise für Filme ändern oft

Nachster Schritt? Überlegungen:
* Was ist unsere Aufgabe?
* Wie passen die Anforderungen ins Gesamtbild?
* Welche Systemteile werden wir berühren und anpassen müssen?
* Wo liegen die interessanten Grenzfälle?
* Welche Klassen und Methoden benötigen wir?
* Wie werden die Klassen benutzt?
* Wie können wir das testen?


Fazit: Entwurf Klassenschnittstellen
Tool: Code lesen oder UML des Codes oder mit Kollegen besprechen und Skizzieren

Zwei Wege:
* Top-down, von allgemeinen Klassen zu spezifischen Klassen vorarbeiten
* Buttom-up, Teile erstellen bis zum Ganzen

Testgetrieben Entwicklung geht welchen Weg? Kommt drauf an, aber immer vom Bekannten zum Unbekannten.

### 5.15 ... und der wegweisende Test
Meist eine Mischung Top-down und Buttom-up. Oft beginn mit Top-down die Anforderungen erfassen, im Test die Intention festhalten und dadurch die Klassen und Methoden finden. Top-down ist hilfreich damit nur implementiert wird was wirklich gebraucht wird.
Oft geht es dann über zu Buttom-up. Um die übergeordneten Strukturen aufzubauen.

Grosse Aufgabe braucht einen grossen Test. Der Test stellt sicher das die Aufgabe auch verstanden wurde. Stellt somit mein Verständnis auf die Probe und die Entwicklung erhält eine Richtung. Achtung dieser erste Test ist mehr Integrationstest als Unit Test. Bezieht sich also aufs Zusammenspiel mehrer Objekte.

Test Bsp:
''' Java
class CustomerTest {
	private Customer customer;
	private Movie buffalo66, jungleBook, pulpFiction;
	
	@BeforeEach
	public void setUp() {
	customer = new Customer();  //Instanz mit Customerkonstruktor erstellen
	buffalo66 = new Movie("Buffalo 66", Price.NEWRELEASE);
	jungleBook = new Movie("Jungle Book", Price.REGULAR);
	pulpFiction = new Movie("Pulp Fiction", Price.NEWRELEASE);
	}
	
	@Test
	@Description("testPrintingStatement")
	public void testPrintingStatement() {
		customer.rentMovie(buffalo66, 4);
		customer.rentMovie(jungleBook, 1);
		cusotmer.rentMovie(pulpFiction, 4);
		
		buffalo66.setPrice(Price.REGULAR);
		
		String actual = customer.printStatement();
		String expected = "\tBuffalo 66\t3,00\n"
				+ "\Jungle Book\t1,50\n"
				+ "\tPulp Fiction\t5,50\n"
				+ "Gesamt: 10,00\n";
		asserEquals(expected, actual);
	}
	...
}
'''

Was bringt dieser Test? Er ist ein nützliches Designwerkzeug. Er hilft Entwurfsentscheidungen zu treffen.
* Umfang begrenzt, schreibe den Test so das klar ist was zu tun ist und was nicht
* Startpunkt setzten, beginne mit ein oder zwei bekannten oder offensichtlichen Klassen
* Kontext beachten, wichtig sind nur aktuelle Probleme von diesem Test, nicht was noch kommen könnte
* Szenarien durchspielen, beginn mit Bekanntem, gehe später zu Was-wäre-wenn-Betrachtung zu Unbekanntem
* Mitwirkende Objekte finden, Problem in Teilprobleme auflösen
* Verantwortlichkeiten verteilen, auf vorhandene oder neue Klassen
* Verhalten spezifizieren, Protokoll der Klassen definieren, via Eingabe zur Ausgabe im Test in Beziehung setzen

Abgeleitetes Design:
''' Java
public class Movie... 
 public Movie(String title, Price price) {
 }
 public void setPrice(Price price) {
 }
}
public class Customer...
 public void rentMovie(Movie movie, int daysRented) {
 }
 public String printStatement() {
 return null;
 }
}
'''
JUnit Test: Schlägt fehl. Genau was erwartet wird.

Fazit: Der Test bietet eine Lernumgebung, um im engen Feedbackzyklus mit dem Code mit altenativen Designideen zu experimentieren. => Fork mit Git machen.

### 5.16 Fake it (’til you make it)
Kent Beck ist der Gedankenverbreiter, Erwartung ausgeben und dann solange programieren bis Resultat der Ausgabe entspricht:
''' Java
public class Customer...
 public String printStatement() {
 return "\tBuffalo 66\t3,00\n"
 + "\tJungle Book\t1,50\n"
 + "\tPulp Fiction\t5,50\n"
 + "Gesamt: 10,00\n";
 }
}
'''
Junit Test: grün! Nun lassen sich konstante Werte durch Objekte ausdrücken. Diese Technik erleichtert selbst komplizierte Aufgaben. Zudem stellt das Erfüllen des Tests mit Objekten sicher, das alle nötige programmiert wurde.

Während dieser Generalisierung, erinnere dich dies ist eher ein Integrationstest, stossen wir auf Codestellen, für die wir weitere Unit Tests schreiben müssen. Wir treiben die Entwicklung der verallgemeinerten Lösung also durch neue Tests an, wo es sinnvoll ist.

In diesem Fall leiten wir die Testliste aus dem geschriebenen Code ab, fast eins zu eins:
* Movie: Filmtitel
* Move: Berechnung verschiedener Preise
* Movie: Reklassifizierung

Die Preissummierung sollte wie gehabt funktionieren. (add oder plus Methode die schon besteht? Nein Customer.getTotalCharge)

### 5.17 Vom Bekannten zum Unbekannten

Die Reihenfolge der Testfälle ist entscheidend. Womit anfangen? Den Test *testTitle* ist überflüssig. Get- und set-Methoden oder Konstuktoren, die keine Logik enthalten werden nicht getestet.

Movie getCharge Methode
''' Java
public class MovieTest extends TestCase { JUnit: OK
 public void testUsingNewReleasePrice() {
 Movie movie = new Movie("Fight Club", Price.NEWRELEASE);
 assertEquals(new Euro(3.75), movie.getCharge(3));
 }
}
'''
Wer verwendet die getCharge Methode? Klasse Customer. Müsste an mehreren Stellen geändert werden, so wäre Schnittstellenevolution anzuwenden.

Problem: Customer Objekt hat kein Movie Objekt als Argument und kennt weder Filmtitel noch -preis um es sich selbst zu erzeugen. Die rentMovie Methode haben wir bereits mit der Signatur überladen, die ist aber noch leer, Kap 5.15.

Damit wir nicht noch eine Baustelle aufmachen, gehen wir den Weg rückwärts. Dies führt zu hässlichem Code, Notiz 'XXX Code bereinigen' nicht vergessen:
''' Java
public class Customer...
 public void rentMovie(int daysRented) {
 Movie movie = new Movie(null, null);
 totalCharge = totalCharge.plus(movie.getCharge(daysRented));
 }
}
getCharge in non static umwandeln:
''' Java
public class Movie...
 public Euro getCharge(int daysRented) {
 return Price.NEWRELEASE.getCharge(daysRented);
 }
}
'''
Beide rentMovie Methoden verbinden:
''' Java
public class Customer...
 public void rentMovie(Movie movie, int daysRented) {
 totalCharge = totalCharge.plus(movie.getCharge(daysRented));
 }
 public void rentMovie(int daysRented) {
 Movie movie = new Movie(null, null);
 rentMovie(movie, daysRented);
 }
}
'''

Jetzt lässt sich im Test ein Movie Objekt verwenden:
''' Java
public class CustomerTest...
 public void testRentingOneMovie() {
 customer.rentMovie(pulpFiction, 1);
 assertEquals(new Euro(2.00), customer.getTotalCharge());
 }
}
'''
Sobald alle rentMovie Testfälle umgestellt sind, kann die hässliche rentMovie Methode entfernt werden.

Wie weiter? Movie Objekt erhält Zustand 'title' und 'price':
''' Java
public class Movie... JUnit: OK
 private Price price = Price.NEWRELEASE;
 public Euro getCharge(int daysRented) {
 return price.getCharge(daysRented);
 }
}
'''

So wurde die Price Instanz fest kodiert, deshalb muss erst ein Test entstehen welcher die andere Variante testet.
''' Java
public class MovieTest... JUnit: Failure
 public void testUsingRegularPrice() {
 Movie movie = new Movie("Brazil", Price.REGULAR);
 assertEquals(new Euro(1.50), movie.getCharge(3));
 }
}
'''
Lösung:
''' Java
public class Movie... JUnit: OK
 private Price price;
 public Movie(String title, Price price) {
 this.title = title;
 this.price = price;
 }
}
'''
Nächster Test Preiswechsel bei einem Film:
''' Java
public class MovieTest... JUnit: Failure
 public void testSettingNewPrice() {
 Movie movie = new Movie("Brazil", Price.NEWRELEASE);
 movie.setPrice(Price.REGULAR);
 assertEquals(new Euro(1.50), movie.getCharge(3));
 }
}
'''
Dieser Test ist an und für sich sich keinen Test wert. Der Test ist aber wichtig weil er ausdrückt, dass ein Film seine priesklasse wechseln kann!
Lösung:
''' Java
public class Movie...
 public void setPrice(Price price) {
 this.price = price;
 }
}
'''

Ausleihe speichern, damit der letzte Preis erscheint und nicht der aktuelle:
''' Java
import java.util.*;
public class Customer...
 private List rentals = new ArrayList();
 public void rentMovie(Movie movie, int daysRented) {
 totalCharge = totalCharge.plus(movie.getCharge(daysRented));
 rentals.add(new Rental(movie, daysRented));
 }
}
'''
Daraus entsteht die Assoziationsklasse/-objekt Rental:
''' Java
public class Rental {
 public Rental(Movie movie, int daysRented) {
 }
}
'''
Rental Test, testet wie lange ein Kunde welchen Film geliehen hat:
''' Java
public class RentalTest extends TestCase {
 public void testUsingMovie() {
 Movie movie = new Movie("Blow-Up", Price.NEWRELEASE);
 Rental rental = new Rental(movie, 2);
 assertEquals(new Euro(2.00), rental.getCharge());
 }
}
public class Rental...
 public Euro getCharge() {
 return null;
 }
}
'''

Um die Kosten zu berechnen delegiert Rental an Movie:
''' Java
public class Rental { JUnit: OK
 private Movie movie;
 private int daysRented;
 public Rental(Movie movie, int daysRented) {
 this.movie = movie;
 this.daysRented = daysRented;
 }
 public Euro getCharge() {
 return movie.getCharge(daysRented);
 }
}
'''

Gesamtbetrag neu gemäss Liste von Rental Objekten:
''' Java
public class Customer... JUnit: OK
 public Euro getTotalCharge() {
 Euro result = new Euro(0);
 for (Iterator i = rentals.iterator(); i.hasNext(); ) {
 Rental rental = (Rental) i.next();
 result = result.plus(rental.getCharge());
 }
 return result;
 }
 '''
 
Fake Konstanten mit Variablen ersetzen:
''' Java
public class Customer... JUnit: Failure
 public String printStatement() {
 return "\tBuffalo 66\t3.00\n"
 + "\tDas Dschungelbuch\t1.50\n"
 + "\tPulp Fiction\t5.50\n"
 + "Gesamt: " + getTotalCharge() + "\n";
 }
}
'''
JUnit: Fehler. Warum? Format stimmt eben nicht überein 10.00 vs. 10.0
Lösung: Rückwärts programmieren, also so tun ob es die Funktion format schon gäbe.
''' Java
public class Customer...
 public String printStatement() {
 return "\tBuffalo 66\t3.00\n"
 + "\tDas Dschungelbuch\t1.50\n"
 + "\tPulp Fiction\t5.50\n"
 + "Gesamt: " + getTotalCharge().format() + "\n";
 }
}
...
public class Euro...
 public String format() {
 return null;
 }
}
...
public class EuroTest...
 public void testFormatting() {
 assertEquals("2,00", two.format());
 }
}
'''
JUnit: zwei Fehler. Hier ist es ok zwei Tests auf einmal offen zu haben, weil wir genau wissen das diese zusammenhängen. Wenn einer läuft, läuft der andere:
''' Java
import java.text.NumberFormat; JUnit: OK
public class Euro...
 public String format() {
 NumberFormat format = NumberFormat.getInstance();
 format.setMinimumFractionDigits(2);
 return format.format(getAmount());
 }
}
'''
JUnit: grün.
Weiter mit Implementierung bei Customer printStatement:
''' Java
public class Customer...
 public String printStatement() {
 String result = "";
 for (Iterator i = rentals.iterator(); i.hasNext(); ) {
 Rental rental = (Rental) i.next();
 result += "\t" + rental.getMovieTitle()
 + "\t" + rental.getCharge().format() + "\n";
 }
 result += "Gesamt: " + getTotalCharge().format() + "\n";
 return result;
 }
}
...
public class Rental... JUnit: OK
 public String getMovieTitle() {
 return movie.getTitle();
 }
}
'''

Noch weitermachen?

