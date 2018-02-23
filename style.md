# Styles and problems
## Fizz Buzz
### My solution:
''' JAVA
		if (teilbarDrei && teilbarFuenf == true) {
			return "Fizz Buzz";
		} else if (teilbarDrei == true && teilbarFuenf == false) {
			return "Fizz";
		} else if (teilbarDrei == false && teilbarFuenf == true) {
			return "Buzz";
		}
		return zahlString;
'''
		
### Solution from Tom Scott:
* https://www.youtube.com/watch?v=QPZ0pIK_wsc)

'''
for loop (i = 1, i <= 100; i++) {
String output = ""; // create an empty output

if (i % 3 == 0) {output = output + "Fizz"} // add Fizz to output if dividable by 3
if (i % 5 == 0) {output = output + "Buzz"} // add Buzz to output if dividable by 5

if (output == "") {output = i} // if output is none of the above write the number instead
}
'''