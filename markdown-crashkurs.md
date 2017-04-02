# Markdown Crashkurs
## Was ist Markdown?
Ein Sprache um Text einfach zu erweitern. Mit Markdown lässt sich Text durch Zeichen und Symbole formatieren. Markdown kann bei github in Gists, Kommentaren und Files mit `.md` oder `.markdown` verwendet werden.
 
 ## Beispiele Textformatierung
 ### Fett, Kursiv und Links
- **Fett** entspricht `**einem Text mit zwei Sternen am Anfang und am Ende**`.
- *Kursiv* ist `*das Geliche mit einem Stern*`. 
- Links sind entweder die URL direkt abgeschrieben oder `[Linkname](URL)` [Name Link **gefolgt** von einer URL](http://bsp.com) 
 
 Weiter können Listen, Bilder, Überschriften und Zitate, Code und Extras eingebunden werden. Eine Anleitung ist unter https://guides.github.com/features/mastering-markdown/ einsehbar.
 
## Interessante Textformatierungen
### Text ohne Formatierung anzeigen, also Markup ausschalten?
Um eine \*Textformatierung\* von Markup zu umgeben wird vor *jedem* Zeichen ein Backslash eingeführt. Beispiel `\*nicht Fett\*`. 

### Backticks
Backticks `erstellen diese Formatierung` ein Backtick wird auf meiner Tastur mit `Shift + ^` erstellt.

### Liste
Task Listen
- [ ] Task1: Task Felder werden mit `- [Leerzeichen]`erstellt.
- [x] Task2

### Code
Code wird als Code angezeigt wenn vier Leerzeichen am Anfang der Zeile verwendet werden `   Code wird ...`.
    
    Vier Leerzeichen
    
Soll noch die Sprache erkannt werden mit Syntaxmarkierung, dann die Zeile mit drei Backticks beginnen gefolgt vom Namen der Sprache ` Java` als Beispiel.
    
``` Java
int a = 0; // Java Beispiel
```
