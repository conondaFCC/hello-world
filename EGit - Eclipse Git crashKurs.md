# EGit - Eclipse Git crashKurs.md
* According to guide http://wiki.eclipse.org/EGit/User_Guide
* Own guide see Git project JAV-ZF-II README.md which shows an integration of Eclipse project into GitHub.
* Understanding of creating a git version Repository
* Understanding of creating a Eclipse Java project
* Does the Understanding of Git and Eclipse result to best practice?

## Understand Git, creating a git Repository
see git.md for this.

## Reload/add on older Eclipse Project from a git repository
### a git repository contains one or more Eclipse Projects
git pre work:
* have your repository up to date aka the version you want do use/restore.

Steps in Eclipse:
* 'Open Perspective' git
* add the git repo via add git repo button (if this is the first git repo to add, git view will tell you this and give the option to add one).
* In git view right click on Working Tree for the option 'Import Projects'. Follow on screen directions.
 
### Multi Eclipse Projects in one or more git repositories
Reload the root git project, all the sub gits should be found correctly.
Also see post with same question: https://www.eclipse.org/forums/index.php/t/1078765/

## Create a GIT branch inside Eclipse
If you know GIT CLI, you might wanna do it there instead (Git Shell > PowerShell on Windows).

* Goto GIT perspective
* Open GIT Repository project under GIT three
* Under > Branches > Local find tree to branch then via right click option branch three (note this makes a local branch, not visible online until pushed!)
* Merging jet to be tested
* Check on what branch are we working? > project folder in the Java Perspective shows the branch name in ().

## Local Branch vs Remote Branch
Eclipse shows the local and the remote branches in the git view under branches. Standard for remote branches, a clone will set its remote to the origin. The origin does not have the remote link to the clone by default. Can be added, see git.md.
