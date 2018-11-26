# PowerShell
## learn about PowerShell
* PowerShell main website: https://docs.microsoft.com/en-us/powershell/
* other tutorial sites:
  * beginner guide: https://programminghistorian.org/lessons/intro-to-powershell
  * Windows PowerShell Survival Guide: https://social.technet.microsoft.com/wiki/contents/articles/183.windows-powershell-survival-guide.aspx
  * Link to Short-keys: https://docs.microsoft.com/en-us/powershell/scripting/core-powershell/ise/keyboard-shortcuts-for-the-windows-powershell-ise?view=powershell-6

## PowerShell commands
| key           			| Detail         |
| :-------------------------| :------------- |
| $                 | access built in variables '$PsVersionTable' |
| Ctrl+C 	    			| abort command (Ctrl+c UNIX bash)   	|
| Q							| exit a List (git diff for example)	|
| fc filename   			| create file (UNIX touch filename) 	|
| echo "a b" > file  		| create file with String 'a b'  		|
| echo "c d" >> file  		| append String to file         		|
| echo "a " "b " > file 	| creates file with two lines of Strings  |
| type file1 file2 > file3  | create file with combined texts  		|
| type file1    			| writes content to console  			|
| cat *.md >> myLogFile.log | apends all *.md contents				|
| rm file1      			| removes file1             			|
| rm folder     			| removes folders           			|
| rm folder -force   		| rm folder (hidden and read-only files)  |
| rni oldname newname 		| rni = rename-item, renames file or folder  |
| ls *.txt					| lists all items *.txt in this folder	|
| Get-command get-* | lists all commands that have the get command first |

## How to open PowerShell in this Folder from Explorer?
Long answer: - Write a little script to get right click option, see: https://www.addictivetips.com/windows-tips/open-powershell-in-a-specific-location/
Short answer: - Navigate to folder in Explorer then type powershell in path and hit enter.

## How to diplay %PATH% - aka linked programs aka call program by exe name from anywhere
$echo %PATH%
### Where is this file under Windows?
Find it via Windows-key 'Environment variables' under the tab advanced, find 'Environment variables'

## PowerShell scripting
**Note: to run scripts you need special privileges like 'powershell.exe -ExecutionPolicy Unrestricted' or others.**
See @'About Execution Policies'on Windows PowerShell help.
