# git Manual and notes

## what is git?
Git can track changes in files and folders, take snapshots, branch and merge different snapshots and revert to older ones. In one word a very capable version control system. Maybe the best one around.
A deeper look at git: https://git-scm.com/book/en/v1/Git-Internals-Plumbing-and-Porcelain

## git Tutorial and help
To understand git, go throuhg these 3 links:
* Don't just read it, **do the examples in it, then you know how to use git!**
* Read the chapter 'description' on this link: https://git-scm.com/docs/git That one is easy!
* do the git tutorial: https://git-scm.com/docs/gittutorial
* do the git daily: https://git-scm.com/docs/giteveryday
* at the very least do some of git tutorial and fly over git daily and read this paragraph https://git-scm.com/docs/gitworkflows#_separate_changes!

## **switching from a branch to another, implications**
Be aware that switching from a branch to another branch, inside a git project, will change the files and folders to it's current state. It will physically alter files according to the branch's state. This might sound normal and logical or not, but it is true and important to know.

## content of this document
mostly this is a recapitulation of the git tutorial with some comments.

## git cheatcheat
PDF: https://education.github.com/git-cheat-sheet-education.pdf

## git on Windows
On windows use the Git Shell from the GitHub Client via PowerShell. Very good CLI tool for windows! Has more information when working with a git repository than the linux bash! The information is even color coded. (How to get this info in bash?)

``` PowerShell
// Git Shell from GitHub Client on Windows PowerShell, has info [branch, files, changes, more..]
~\Desktop\testGit [experimental +1 ~1 -0 !]> git add .
~\Desktop\testGit [experimental +1 ~1 -0 ~]>
```
* NOTE: File and folder Path on Windows is with backslash \ whereas inside git, for example branches it will be 'git merge bob/master' with a normal slash.

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
* more on merge follow git remote, fetch and merge below

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

  This will show what files have differences. For more info on the differences:
  ``` PowerShell
  $ gitk HEAD..FETCH_HEAD // or
  $ gitk HEAD...FETCH_HEAD
  ```

[ ]* save stash, pull then unstash over the pull.. explanation needed

## define remote repository, fetch and merge on remote
Define a remote repository by giving it a short name and the project path.
``` PowerShell
alice$ git remote add bob /home/bob/myrepo
// bob = give remote repository a name
// /home/bob/myrepo = remote sourcePath
```
Fetch a set remote by short name. This will make a new branch! -> (remoteName/branchName)
``` PowerShell
~\git\alice [master]> git fetch bob
From ..\bob
 * [new branch]      master     -> bob/master
```
compare the local branch with the remote branch
``` PowerShell
alice$ git log -p master..bob/master
alice$ git log -p HEAD..FETCH_HEAD // same result
```
merge the two Branches, two almost identical options:
``` PowerShell
alice$ git merge bob/master
alice$ git pull . remotes/bob/master
```
** WARNING ** git pull = will merge in current branch, even if told otherwise in CLI!!

## remarks for cloned repositories
* a cloned repository has a set remote
* therefore a 'git pull' command can be given without defining a remote to pull from
* the clone keeps a copy of the remote/master
``` PowerShell
bob$ git branch -r
>>> origin/HEAD -> origin/master
>>> origin/master
```

## git push
git push is meant to push to remote repository aka clones or defined remotes. Pushing to an origin might give an error. Mostly because the branch to push to is checked out. There are different solutions to this:
1. send pull request to origin or pull directly from origin
2. work around the error, see git pull --help or web, **only do this if clear what you are doing!** in origin detach branch 'git commit --detach branchName'

## exploration of history
More details on exploring the history of commits under: https://git-scm.com/docs/gittutorial#_exploring_history
Some topics there are:
* $ git show HEAD		# the tip of the current branch with diff
* $ git branch -r		# list remote-tracking branches
* tag system, tag a commit with a String.
Example: ''$ git tag v2.5 1b2e1d63ff'
* git reset is shown
* 'git log stable..master' not = 'git log master...stable'!!
* git log has weaknesses, gitk might do a better job

## git problems and solutions
### git push -  ! [rejected]        master -> master (non-fast-forward)
push to origin is rejected, try git fetch origin, then git merge origin currentBranch
or both in one step, git pull origin currentBranch
Solution: https://help.github.com/articles/dealing-with-non-fast-forward-errors/

## OLD NOTES, NEEDS REVISION:
## git create a new repository
what does it mean create a new version controlled system? It means creating a folder with some hidden files in it. Inside the new folder there will be a .git folder containing mostly all files needed to handle a git Repository. One example is the 'config' file, which tells what the kind of git Repository it is. From Git Man Page:
* bare
  * Make a bare Git repository. That is, instead of creating <directory> and placing the administrative files in <directory>/.git, make the <directory> itself the $GIT_DIR. This obviously implies the -n because there is nowhere to check out the working tree. Also the branch heads at the remote are copied directly to corresponding local branch heads, without mapping them to refs/remotes/origin/. When this option is used, neither remote-tracking branches nor the related configuration variables are created.

* mirror
  * Set up a mirror of the source repository. This implies --bare. Compared to --bare, --mirror not only maps local branches of the source to local branches of the target, it maps all refs (including remote-tracking branches, notes etc.) and sets up a refspec configuration such that all these refs are overwritten by a git remote update in the target repository.

If creating it with GitHub client the options are limited, for more options use CLI or Git Shell (comes with GitHub Client).
