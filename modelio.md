# modelio
User manual: http://forge.modelio.org/projects/modelio3/wiki
http://forge.modelio.org/projects/modelio3-usermanual-english-300/wiki

## Setup of Modelio and work chain
### Setup
#### Standard views, add JDK library
* to see class attributes and operations (methods) in diagrams a two step setup: 1. set up a new 'Diagram theme' under configuration > diagram themes > create new diagram theme button (name example: yourNewDiagramTheme) then activate the boxes under > class > attributes and operations > show for your new theme; 2. select diagram in model browser tree, change diagram theme to yourNewDiagramTheme in Symbol tab (right edge of screen), maybe you need to change each class too, for this select the class in the diagram then open Symbol tab , select 'Element Style = default or yourNewDiagramTheme' + select 'Representation Mode = Structured'
* show initial values for attributes in diagrams, not yet possible: http://forge.modelio.org/issues/334
* Goto Configuration > Library > add from local file if already downloaded or add from update > select JDK https://www.modelio.org/forum/7-general-help/4251-attribute-datatype-from-java.html?start=6

### Modelio & Eclipse Round Trip work chain
* create new project in Eclipse
* Create new java project in Modelio, *don't give it the same name like an possible package name in Eclipse (com.dontusethatname.java)*
* set path to Eclipse package (this will be the source for the .java files), go to Configuration > Modules >  Java Designer, select directories > code generation path to location of Eclipse project > package
* synchronise package folder (see more detailed explanation below when unclear)
* model classes with attributes, constructors and methods aka add class elements etc. to the package, then save
* create java files via package, select Java Designer > generate
* switch to Eclipse, write code for existing Methods and constructors, save files
* switch to Modelio, update, select package > Java Designer > update model from source
* add new classes, attributes, constructors and methods
* overwrite java files via package, select Java Designer > generate
* repeat from switch to Eclipse.

#### synchronise the package folder
in order for round trip to work or to have the package from the Eclipse project show up in Modelio synchronise the package (folder) of both Eclipse and Modelio
When no files (or only .java files in Eclipse) have been made:
* Remember a new Eclipse project will generate one src package, create an additional new package and save it (with or without .java files)
* go to Modelio, one standard package (=src package from Eclipse) has been created by Modelio, select it and update from source to import the Eclipse package and any .java files.

#### Backup/export a modelio project?
In the modelio main screen aka workspace view, right click the project, then select export.

## tips
### create UML class diagram with or without JAVA code
Use the Modelio software which is based on Eclipse. https://www.modelio.org
In Modelio a Java project can be imported and turned into a UML class diagram or the other way round a diagram can be made into JAVA code! (This will not set the generate path back to the source .java files, only one way setup Eclipse to Modelio! See roundtrip for both ways setup)

1. open modelio and create new project (Ctrl+N)
2. Set project to **java project** on creation!!!
3. on the created project in the tree view click import a model
4. on the last new folder click > Java designer > Reverse > reverse Java from source
5. select src folder (Eclipse workspace > package)
6. select .java files hit reverse
7. right click the package > create diagram > choose one (for UML class diagram its package structure diagram automatic)
8. adjust display setting of class to show attributes and operations by default (Configuration > Diagram themes > default > class > Attributes and Operations enable show)
9. update diagram (in Model Browser window via right-click > Automatic Diagram > update automatic diagram)

new workflow:

1. create a new java project in Modelio
2. create a new java packet in Modelio, under the project name standard package (this will be the packet shown under src > yourNewPacket in Eclipse)
model classes with attributes, constructors, and methods in Modelio
3. create a new java project in Eclipse
4. set Modelio Java Designer's generation path to be the same as your Eclipse project src folder. Menu Configuration > Modules > Java Designer > Directories > Code generation path
5. create java files via package, select Java Designer > generate (regardless of generation from class or package, since the package is not yet existing in Eclipse it will be generated as well)
6. in Eclipse add javadesigner.jar, Menu Project > Properties > Java Build Path > Libraries > add external JARs add javadesigner.jar (~\.modelio\3.7\modules\JavaDesigner_3.7.01\JavaDesigner\bin\javadesigner.jar 'To be able to compile outside Modelio, you must add this archive. If you generate the ANT file from Modelio, the Jar archive is added automatically.'
7. in Eclipse, refresh project (F5), write code for existing Methods and constructors, save files
8. switch to Modelio, update, select package > Java Designer > update model from source
9. add new classes, attributes, constructors and methods
10. overwrite java files via package, select Java Designer > generate
repeat from 8.

### create Java code with an existing UML class diagram
In Modelio there is the option to right click a package or a class then select Java designer > Generate. This will create the .java class files for the selected class or package in the Modelio workspace folder.
#### Hints:
* to remove the Round Trip remarks (@objid) in the code aka the .java files, go conf > modules > java designer > general > generation > release
* rename class only in Eclipse (otherwise old class will not be overwritten)

### solve package problems Modelio vs Eclipse
If the sync of packages is messed up like this: in Eclipse 'mypackage.mypackge' when generating .java from Modelio, try this:
* move source of modelio to src of eclipse project
* **worry about overwriting other eclipse files a lot! be very careful when using the generate function from modelio on packages. ideally just use it on the package you work with, never on the src folder** otherwise all .java files might get overwritten with the modelio roundtrip version!
* delete the unwanted packages from the modelio package view to minimize the risk of overwriting all packages. 

### how to set values for class attributes on initializiation
Modelio has a field for each class attribute called value. This field can be filled with a value to represent the standard value of the attribute. If the value is of type string just enter the value with the quotation mark "MyValueWithDoubleQuotationMark". This way it will be represented the right way in the Java code.