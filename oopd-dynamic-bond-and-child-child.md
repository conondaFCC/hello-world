#oopd-dynamic-bond-and-child-child
##destillation from the video overwrite methods
@see also good german page is java-tutorial.org for quick run down of inheritance 'vererbung'

The video shows how to solve one common problem, how to make a new class from an exitsting class, but just some a little changes for the new class, so it might be wise to make a child of child instead of a new child from the super class.

In the example a new from of a rectangle is used which has its corners cut in 45° angles with an offset value. So instead of making a new class for this a child from rectangle is made.

This comes even more usefull when looking at the methods used in rectangle, now these methods can be used on its child too.

In the example a surface area method is shown which calculates the area of a surface. Now the new rectangle has a smaller surface area, so the method can be adapted via the @override anotation. And very smartly the old method is just expanded by subtracting from the rectangle only the cut pieces. So there is no redundant code, the old method is still used but extended! Future proof too.

Then it goes on to show how this method can be added to the super class. Where it can not be used or defined because the element is not known. Therefore the method is made an abstrac method like so: 'abstract int surfaceAera();' this makes the class become abstract too. Now every child needs to implement the surfaceAera() method too. 
The difference between no modifier and public is unclear atm, especially regarding child child behaviour. ** You might want to 2x check that the child child has the method implemented aswell if need be! ** Automatism is unlclear here but in Eclipse can be implemented quick via 'Source > Override/Implement Methods...' option.
AFAIK aka uncertain, especially with child child consequences *Usually the method will be public: 'public abstract void myAbstractMethod();'. So the compiler expects every child to have this method implemented.*


The other interesting topic was the dynamic bond which is the ability to access specific child methods even when just using super class type references. Example: for the rectangle with cut corners the surface aera is smaller. Best see code:
``` Java
CutRectangle cr = new CutRectangle(10, 10, 100, 50; 10);
Rectangle r = cr;
syso(r.surfaceAera()); // this will give the correct surfaceAera since it will access the method of the CutRectangle!
```

** BEWARE adding child of child later on**
Once you have your code logic set up for parents and childs, then later you introduce a child of a child, this might mess up your logic. In the following example, the instanceof logic made it so that for each Rechteckgefuellt it made another Rechteck until I changed the logic and introduced elseif like 'if Rechteckgefuellt do, elseif Rechteck do for Rechteck only'.
Example: grafics editor code
``` Java
private void zeichneFiguren(Graphics g) {
	for (Figur f : figuren) {
		/*  DONE Problem, da nun Rechteckgefuellt ein Kind von Rechteck ist, 
		 *  erhalte ich unerwuenschte zusaetzliche Rechteke im Editor -> Loesung:
		 *  'if Rechteckgefuellt do, elseif Rechteck do for Rechteck only'
		 */
		if (f instanceof Rechteckgefuellt) {
			Rechteckgefuellt rg = (Rechteckgefuellt) f;
			if (f.c == null) {// if Color not set, fill rectangle with Standardcolor
				g.fillRect(rg.x, rg.y, rg.getBreite(), rg.getHoehe());
			} 
			Color current = g.getColor();
			g.setColor(f.c); //else fill with set Color
			g.fillRect(rg.x, rg.y, rg.getBreite(), rg.getHoehe());
			g.setColor(current); // reset Color to current aka Reset Color selection to default
		}
		else if (f instanceof Rechteck) {
			Rechteck r = (Rechteck)f;
			//        g.drawRect(r.getX(), r.getY(), r.getBreite(), r.getHoehe()); // Original code
			g.drawRect(r.y, r.x, r.getBreite(), r.getHoehe()); // adapted code, since x, y are super but only protected attributes
		}
		...
```
		