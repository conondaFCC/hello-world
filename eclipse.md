# Ecslipse Crashkurs
## Basic shortcuts
Good read on more shortcuts and shortcut-Card to print: http://javarevisited.blogspot.ch/2010/10/eclipse-tutorial-most-useful-eclipse.html

Command         |Action
----------------|------
Ctrl+Shift+L    |Show shortcuts
Ctrl+D          |delete row
Ctrl+Shift+UpAr.|duplicate row; or Ctrl+Shift+DownArrow
Alt+up/down     |move row accordingly
Alt+left/right  |move to last (object edited, aka jump to previous method)
move method     |via Outline view drag&drop it (deselect ABC sorting)
Ctrl+Shift+o    |import
Ctrl+1          |quickfix
Ctrl+J          |Search in File (incremental)
Ctrl+Shift+T    |search extended
Ctrl+E          |list open files (to skim trough)
Ctrl+F6         |jump between to open files
Ctrl+F7         |jump between views
Ctrl+F8         |jump between perspectives (java, git)
Ctrl+K          |jump to?
Ctrl+F11        |run app
Ctrl+N          |new wizard
Ctrl+M          |min. max. tab
Ctrl+I          |indent
Ctrl+Shift+F    |cleanup code?
Ctrl+J          |incremental search
Ctrl+Shift+G    |create getter an setter?
Alt+Shift+J     |add JavaDoc comment, works on class, variable or method
Ctrl+7          |Line comment on/off  
Ctrl+O          |quick outline class or twice Ctrl+O all classes in package? Note: Ctrl+O always opens the outline for the current Java editor. Press Ctrl+F3 to open the quick outline for the currently selected type.
Shift+Alt+A     |Block selection aka multiline edit (as on Oxygen.2 Release (4.7.2) this is still not very extendet like in atom, see github stars for project on it)
Shift+Alt+S     |access Source commands like create getter and setter etc.
toString()      |from Source get all fields toString
Ctrl+Shift+Mouse|Ingore warning and show Javadoc anyway
Shift+Mouseover |Shows full declaration of variable
F3              |Jump to Method or Class, also via Ctrl+LMB on method
variable        |If declared use the camelcase abbrevation to write it. Ex.: thisVariable => tV
variable2       |create field from variable with Ctrl+1
variable3       |instead of writing the variable, start with writting the method if the variable is an input of this method (int a, int b), then autocreate those with Ctrl+1 => create field
Ctrl+Space      |Autocomplete for ex.: ThisMethod => creates method
Ctrl+Shift++/-  |Fontsize adjust code size for presentation
Ctrl+r          |While debugging => run to line (ingnore other breaks!)
F8              |jump to next break, while in debugging mode
Alt+Shift+T     |Refactor Options with according code bits selected ex. Variable to Convert Local Variable to Field or whole code bit to Extract Method from it
.jpage         	| Instead of .java .jpage for code snippets aka Scrapbook pages (to run code without main Methods)
Ctrl+Shift++   	| Zoom in (Ctrl+Shift+1 is plus on DEU keyboard)
Ctrl+Shift+-   	| Zoom out(might only work with number block on keyboard, with direct minus sign Shift is not needed! -> Crtl+Minuskey works)
Ctrl+-          | colapse lines of this element into sinlge line
Ctrl++          | expand lines of element to full size aka make content of element readable aka show content of element
Ctrl+H          | open Eclipse search tool
new				      | write 'new' then ctrl+space creates template for new object
ClassName       | write only the ClassName on a new line, then Autocomplete to get a constructor for the ClassName
public          | write 'public' then Autocomplete creates template for new public method

### EMACS or other Controls
Via Windows > Preferences > General > Keys > Schema:

## Eclipse Views
See chapter 'Basic Eclipse Functionality'.

## Eclipse projects (related data)
To understand where eclipse stores data on projects. Most of the data will be stored in the project folder in the files .classpath and .project. Also if the project is not in the default workspace some information (likely the path to the project) will be stored in 'workspace/.metadata'.

## Basic Eclipse Functionality
* **.jpage** to run code snippets (Scrapbook pages) instead of writing a class with a main method just use .jpage. How to use it? Highligt the code inside the .jpage then right click it for options on inspect, run, etc.
* **Help with F1** - open it in a new window once a topic is selected with the corresponding icon.
* **TODO handling** - default view is Resource - to see only TODO from one file > Tasks > 'Triangle' > Show > TODO's instead of show all (all open projects!). The TODO complete option only works for UI generated Tasks! **Must be in corresponding perspectives!! Aka Java perspective.**
* **No Task View?** Open Tasks view via Windows > Show Views > Tasks
* **Each Task View can be coustomized on what content to show! see down arrow on the top right of the current task windows, then 'configure content'. since standard should be show all aka show all defined task see below from all open projects or the current open project. this can be then filtered down via configure content for example to show onl current package or only TODO but not all tasks. again this filter will be lost if the task tab is removed!
* **FIXME and XXX** and DONE > FIXME = bad code not working, XXX = bad code but working, DONE = change tags to when done. Needs to be added by hand via Windows > Preferences > Java -> Compiler -> Task Tags.
* **Save your custom views!** Setup your custom views for eclipse then save them as a perspective.
* **rename a Method in multiple files?** Use the integrated rename tool with Ctrl+1, then use rename in Workspace so Eclipse will handle all the dependencies.
* **generate Constructor using fields** Use class attributes to generate a constructor, rightclick the attribute of a class choose > Source > Generate Constructor using fields > select fields needed for constructor. Note: insert will be on cursor position!
* **generate code, insert on cursor** Most of auto generated code, like Getter and Setters, constructor etc. can be set to an insert location just be having the cursor where you want the code to be generated.
### Eclipse search Tool
open search tool with Ctrl+H.
#### find all occurencies of a method, find method
use java search ribon, search string: method name, search for category: method, scope: enclosed projects
or use Filesearch ribon, search string: method name, scope: workspace, also finds projects outside of workspace aka separate git folder where some git projects are handled outside of workspace, this is good news.

## Compile .java to run in cmd
1. Export file as runnable .jar, open export menu via rightclick on .java file in projectexplorer > select > export > runnablejar
2. Runnable Jar options > Launch configuration, select the one where the to be used main method resides (Needs to be run
  once to be displayed in Eclispe!)
3. lunch cmd - lunch file with: java -jar runnablejar.jar (java runnable.jar didn't work?)

## Compile .java to .exe making a windows .exe file
### with Launch4j-3.7 aka bundle .exe with JRE
Create .jar as explained in 'compile .java ...'.
Install & start Launch4j-3.7, create your exe file.

Options to choose in Launch4j:
Example: 1. tabname >> explanation, tasks
1. Basic >> define Output file: | Jar: where Outputfile is the \\myappfolder\\myapp.exe and Jar is the created .jar from myapp.java
2. Classpath >> do nothing [OLD 2. Classpath >> Select custom classpath: | define Main class: aka locate myapp.jar with main Method]
3. Header >> Select GUI or console app
4. JRE >> point to JRE! this is important copy JRE into myappfolder (myappfolder\\jre1.8...) (example: E:\\minesweeper\\bin\\jre1.8.0_131) then the Bundled JRE Path is: 'bin\\jre1.8.0_131' Example from config-xml file: <path>bin\jre1.8.0_131</path>. The exe is in myapp\\myapp.exe JRE is in myapp\\bin\\jre1.8.0_131 another hint see: https://stackoverflow.com/a/7582064/7698264
5. build and test the app on a system without JRE installed!

The whole explanation is taken from this video: https://www.youtube.com/watch?v=a24PlW-acwY

\\Mermaid graph of process: https://mermaidjs.github.io/mermaid-live-editor/#/view/Z3JhcGggVEQKQVtNeUNsYXNzLmphdmFdIC0tPnx0cmFuc2Zvcm0gaW50byBqYXJ8IEIoTXlDbGFzcy5qYXIpCkIgLS0-IEN7Y2hvb3NlIG9uZX0KQyAtLT58Y3Jvc3MgY29tcGlsZXJ8IERbTGludXgsIEdDQyA-IEdDSl0KQyAtLT58cGFja2FnZXJ8IEVbRXguOiBMYXVuY2g0ai0zLjddCkQtLT5GW0J1aWxkID4gTXlDbGFzcy5leGVdCkUtLT5HW015Q2xhc3MuZXhlICsgSlJFICsgV2luZG93cyBJbnN0YWxsZXJdCkUtLT5IW015Q2xhc3MuZXhlICsgSlJFIExpbmtdCkUtLT5JW015Q2xhc3MuZXhlICsgSlJFXQ
graph TD
A[MyClass.java] -->|transform into jar| B(MyClass.jar)
B --> C{choose one}
C -->|cross compiler| D[Linux, GCC > GCJ]
C -->|packager| E[Ex.: Launch4j-3.7]
D-->F[Build > MyClass.exe]
E-->G[MyClass.exe + JRE + Windows Installer]
E-->H[MyClass.exe + JRE Link]
E-->I[MyClass.exe + JRE]

## Eclipse and Modelio, working together
The new description can be found under modelio.md

** TODO cleanup old Modelio Eclipse Setup below!!!**

### Import Modelio into Eclipse
* Existing Modelio and Eclipse project
* existing files in Modelio (class diagram) and generated class files (converted to class.java files in src folder of Modelio project)

In Eclipse under project explorer right click and select import. Then select File System (files) > browse to file location of .java files > select Options > Advanced > Create link in workspace > Finish
* Creates new package with linked? .java Files
* update possible?

### Change workspace of Eclipse to location of workspace of Modelio
This is recommended by a french guide, http://lipn.univ-paris13.fr/~gerard/uml-s2/uml-td03.html, on how to best work with Modelio and Eclipse together.

### Round Trip synchronised Mode
This mode lets any change in Modelio or Eclipse be synched between each other!

#### Problem with missing JavaDesigner.jar
Fix add JavaDesinger.jar to Eclipse project Java Build path > add external jar > ~\.modelio\3.7\modules\JavaDesigner_3.7.01\JavaDesigner\bin\JavaDesigner.jar

## Glossary
### IDE = EBD
IDE = Integrated development enviroment
EBD = editor, builder, debugger
