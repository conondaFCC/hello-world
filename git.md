# git Manual and notes

## what is git?
Git can track changes in files and folders, take snapshots, branch and merge different snapshots and revert to older ones. In one word a very capable version control system. Maybe the best one around.
A deeper look at git: https://git-scm.com/book/en/v1/Git-Internals-Plumbing-and-Porcelain

## git Tutorial and help
To understand git, do the git tutorial and the git daily from here: https://git-scm.com/docs/git

## git cheatcheat
PDF: https://education.github.com/git-cheat-sheet-education.pdf

## git on Windows
On windows use the Git Shell from the GitHub Client via PowerShell. Very good CLI tool for windows! Has more information when working with a git repository than the linux bash! The information is even color coded. (How to get this info in bash?)

``` PowerShell
// Git Shell from GitHub Client on Windows PowerShell, has info [branch, files, changes, more..]
~\Desktop\testGit [experimental +1 ~1 -0 !]> git add .
~\Desktop\testGit [experimental +1 ~1 -0 ~]>
```

## Get help on a git command
How to understand a command like 'git log --graph'?
``` PowerShell
$ man git-log // doesnt work on PowerShell
$ git help log // help on log command works on PowerShell
$ git commit --help // opens Help on commit in browser on PowerShell
```

## first step, register a user
``` PowerShell
$ git config --global user.name "Your Name Comes Here" // i20100 my githubname
$ git config --global user.email you@yourdomain.example.com //20100@gmx.ch
```

## import or create a git repository
``` PowerShell
$ git init // creates the repository inside the actual folder
```
What files will be in the git repository?
```
Parent folder
  |
  GitProject folder // call git init from here! everything inside and its children will be in git
    | - file1
    | - file2
    More folders
```
* if no project folder exits create one
* if the project already has files call git init from this folder
* for import see https://git-scm.com/docs/gittutorial

## git commands
``` PowerShell
$ git config -l // list the git configuration, without -l print config options
$ git status // get overview of current project, needs to be inside a git folder
$ git add . // add all files from this folder, take snapshot of current state
$ git commit // commit current state, all added to index
$ git add file1 file2 file3 // add specific files for commit, add to index!
$ git diff --cached // Without --cached, git diff will show you any changes made but not yet added to the index.
$ git commit -a // commit all modified, **but not new files**
$ git commit -m "commit text"
$ git commit -m "next commit" -m " " -m "more details on 3rd line" // -m " " empty line by convention
$ git diff // will show
```
a lot more commands are explained under: https://git-scm.com/docs/gittutorial

A note on commit messages: Though not required, it’s a good idea to begin the commit message with a single short (less than 50 character) line summarizing the change, followed by a blank line and then a more thorough description. The text up to the first blank line in a commit message is treated as the commit title, and that title is used throughout Git. For example, git-format-patch[1] turns a commit into email, and it uses the title on the Subject line and the rest of the commit in the body.

## create a branch
``` PowerShell
$ git branch experimental // creates branch experimantal on current git project
$ git branch // shows branches, the * one is the current
$ git checkout experimental // change working branch to experimental, co = checkout
```

## **changing branches and implications**
The changing of a branch has physical implications on the file system and files. Changing from one branch to another will revert the folders and files back or forward to the branches state! Tested this on Windows and Linux!

## git log - show history
``` PowerShell
$ git log // shows git commit history on branch
$ git log -p // with max details
$ git log --stat --summary // with details
```

## git merge
How to merge two branches? Be in master branch.
``` PowerShell
$ git merge experimental // merges experimental branch into master
$ git diff // shows details about conflicting files
// resolve those conflicts, then do git commit -a
$ gitk // shows a graphical result of the history
```

## git delete branch
``` PowerShell
$ git branch -d experimental // will delete experimental branch if changes are merged
$ git branch -D crazy-idea // will delete branch regardless
```
      "Branches are cheap and easy, so this is a good way to try something out."

## git clone
How to clone a repository.
``` PowerShell
$ git clone repositoryPath newRepoName // creates a directory with newRepoName
$ git clone repositoryPath // creates a clone in the current folder, creates a new folder with the name of the old repository
$ git clone alice bob // clones the repository alice into the repository called bob
view of Git Shell:
~\git\alice [master]> cd ../bob
~\git\bob [master ≡]>
```
The clone will point to the repositoryPath as origin.
``` PowerShell
$ git config -l // lists all config parameters
```

## git collaboration, pull and push
After Bob has made some changes to his clone and commited them Alice can pull:
``` PowerShell
~\git\alice [master]> git pull ..\bob master// git pull sourcePath branchName
```
* Pull = get files from sourcePath and merge them into currentBranch
* **WARNING** local changes made from Alice will be overwritten if the were not commited by Alice before the pull! You have been warned!

* **Save peek into Bobs master:** instead Alice can just make a fetch, this means peek into bob's changes:
``` PowerShell
~\git\alice [master]> git fetch ..\bob master //fetch bobs changes on the master branch
~\git\alice [master]> git log -p HEAD..FETCH_HEAD // compare the two master branches
```
NOTE '..' is not = '...' see: https://git-scm.com/docs/gittutorial#_using_git_for_collaboration

  This will show what files have differencies. For more info on the differencies:
  ``` PowerShell
  $ gitk HEAD..FETCH_HEAD // or
  $ gitk HEAD...FETCH_HEAD
  ```

## git create a new repository
what does it mean create a new version controlled system? It means creating a folder with some hidden files in it. Inside the new folder there will be a .git folder containing mostly all files needed to handle a git Repository. One example is the 'config' file, which tells what the kind of git Repository it is. From Git Man Page:
* bare
  * Make a bare Git repository. That is, instead of creating <directory> and placing the administrative files in <directory>/.git, make the <directory> itself the $GIT_DIR. This obviously implies the -n because there is nowhere to check out the working tree. Also the branch heads at the remote are copied directly to corresponding local branch heads, without mapping them to refs/remotes/origin/. When this option is used, neither remote-tracking branches nor the related configuration variables are created.

* mirror
  * Set up a mirror of the source repository. This implies --bare. Compared to --bare, --mirror not only maps local branches of the source to local branches of the target, it maps all refs (including remote-tracking branches, notes etc.) and sets up a refspec configuration such that all these refs are overwritten by a git remote update in the target repository.

If creating it with GitHub client the options are limited, for more options use CLI or Git Shell (comes with GitHub Client).
