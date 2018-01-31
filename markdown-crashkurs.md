# Markdown Crashkurs
## Was ist Markdown?
Ein Sprache um Text einfach zu erweitern. Mit Markdown lässt sich Text durch Zeichen und Symbole formatieren. Markdown kann bei github in Gists, Kommentaren und Files mit `.md` oder `.markdown` verwendet werden.

 ## Textformatierungen
 Anleitung unter https://guides.github.com/features/mastering-markdown/.

 ### Fett, Kursiv und Links
- **Fett** entspricht `**einem Text mit zwei Sternen am Anfang und am Ende**`.
- *Kursiv* ist `*das Geliche mit einem Stern*`.
- Links sind entweder die URL direkt abgeschrieben https://link.com oder
- der `[Linkname](URL)` [Name Link **gefolgt** von einer URL](http://bsp.com)

### Textparagraphe
Um normalen Text in Paragraphe zu trennen muss eine Leerzeile zwischen beiden Textabschnitten liegen.

### Text ohne Formatierung anzeigen, Markup ausschalten?
Um eine \*Textformatierung\* von Markup zu umgehen wird vor **jedem** Zeichen ein Backslash eingeführt. Beispiel `\*nicht Fett\*`.

### Backticks
Backticks `erstellen diese Formatierung`. Ein Backtick wird auf der CH-Tastur mit `Shift + ^` erstellt.

### Liste
#### Task Listen
- [ ] Task1: Task Felder werden mit `- [Leerzeichen]`erstellt.
- [x] Task2

#### Normale Liste
- Mit -  oder * und mit Zahlen (1. usw.) für Zahlenliste
  - Unterpunkte in der Liste mit zwei Leerzeichen davor

### Code
Code wird als Codeblock angezeigt wenn vier Leerzeichen am Anfang der Zeile verwendet werden.

    Vier Leerzeichen

Soll noch die Sprache erkannt werden mit Syntaxmarkierung, dann die Zeile mit drei Backticks beginnen gefolgt vom Namen der Sprache ` Java` als Beispiel.

``` Java
int a = 0; // Java Beispiel
```
### Tabellen
#### Codeanzeige
``` md
Tablehead   |Tablehead2
------------|----------
Entry       |Entry2
```
#### .md-Anzeige

Tablehead   |Tablehead2
------------|----------
Entry       |Entry2
