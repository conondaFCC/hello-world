# java awt drawString font and size
## how to change the fontsize of a string in a GUI? AWT style
``` Java
if (f instanceof Text) {
	g.setFont(new Font(null, 0, 20)); // 20 = Fontsize, null = Font aka here standard is taken
	Text t = (Text) f;
	g.drawString(t.getText(), t.x, t.y);
	}
````