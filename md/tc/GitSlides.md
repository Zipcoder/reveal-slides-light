# Git

-
-

##What Is Git?
According to Wikipedia:

> Git is a version control system (VCS) for tracking changes in computer files and coordinating work on those files among multiple people. It is primarily used for software development, but it can be used to keep track of changes in any files.

-
-

##Basic Git Concepts

-
###Repository

The overall package or project.  There is typically a remote or origin repository, and a local copy of it at a certain time on your local machine.
-
###Branch

An extension of a repository.  All repos start with a `master` branch.  From there, however, you can create a new branch.  This is typically done to keep master clean and working, and give you a place to work on new features.
-
###Commit

A commit is a change to the repository.  For every commit made, you have a record of the state of the repository at that given point in time.

-
-

##Git Commands
-
* `git init`
    * <span class="fragment fade-up">Creates a new local repository in the current directory.</span>
    * <span class="fragment fade-up">This command will rarely be used during this course as most work being done will be collaborative and pre-existing assignments.</span>
    * <span class="fragment fade-up">If you find yourself typing this command regularly, you're probably doing something wrong.</span>

-
* `git clone {project}`
* Clones a repository from a remote resource (Github, Gitlab, internal Git server, etc).
* This will create a directory inside your current directory which encapsulates the project retrieved from the clone command.
* By executing `git clone https://github.com/zipcoder/git-tester.git` in the `dev` directory, one can expect to see a `git-tester` directory generated in their `dev` folder.

-
Running `git status` from `dev` should return 

> fatal: Not a git repository (or any of the parent directories): .git

But, if you go into the `GitTester` directory, it should return:

> On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean

-
* `git status` -- This is the command you will run the most.  What it does is tell you what files you've added, deleted, and modified.  It will also tell you if your local branch is behind the remote version, or if your version is ahead of the remote repository.  If it's behind, then you need to pull or merge the remote repository into yours, and if it's ahead, then you need to push your changes.

-
* `git branch {branchname}` -- This creates a new branch on your local machine.  It does not check out the branch, nor does it put it on the server.
* `git branch -a` -- This shows all branches, both local and remote, that are available.
* `git checkout {branchname}` -- This sets your current, local branch to that given branch.

-
* `git add {file, directory, ., or other}` -- This stages changes to be committed.  This, however does not commit that change.  This essentially means that you're about to commit some changes.  By specifying a file, it will just add that one file.  By specifying a directory, every change from that directory down will be added.  By specifying `.`, that says "Add all changes from this directory downward."  Meaning that if you're in the root of a git project (like `dev/GitTester` from our example above, all changes in `GitTester` and every directory within it will be added.

-
* `git commit -m "{commit message}"`
    * Commits your added changes locally and attaches the commit message to the commit.
    * Commit messages should be short but descriptive of all the changes you've made.
    * `commit` does not apply affect a remote repository until a `push` occurs.



-
* `git push`
    * This will take the locally committed changes for a given branch and apply them to the remote repository.
    * If you've committed a change with `git commit`, the only way for everyone else to be able to use your changes is to run `git push`.




* `git push -u origin {branch}`
    * Creates a new branch on the remote repository.
    * If you create a branch, the server has no knowledge of it.
    * It, essentially, tells the remote repository
        * "Hey, I'm gonna be making changes on a branch, but I want you to keep track of those, too. So here's where to put it and what to call it."

-
* `git pull`
    * This takes the changes that have been made to the remote repository and applies them to your local version.
    * If someone else pushed a change to a remote repository, you must run `git pull` to be able to view and use their changes.

-
* `git merge {branch}`
    * Applies all commits from specified branch to your current branch.






-
-

## Example Time

-
* The commands might not make a bunch of sense without context.
* So here's some context of a workflow that uses git and makes use of the commands.



-
* Consider that you've started working for a new company that is building a calculator.
* They have most of the functionality built out, but they need you to implement the division function of the calculator.
* So, you have some directory called `dev` where you do all your development work.
* Your first step is:
    * `git clone https://git.zipcode.rocks/ZipCodeWilmington/CR-Calcul8r.git`



-
* Now, `dev` has a `Calul8r` directory.
* Executing `git status` in `dev` reveals that there is no git project in `dev`.
* Now, you go into `Calcul8r` and do a `git status` to prove that you're up to date with the master branch of the company's `Calcul8r` repo.




-
Next, you run `git branch -a` to see exactly what branches exist on this companies repository.

> \* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/add
  remotes/origin/subtract
  remotes/origin/multiply


-
* So, from here, you can see that your new colleagues have made branches for
    * `add`, `subtract`, and `multiply`.
* Because you're implementing **divide**, you should make a `divide` branch.
* To do this, you run:
    * `git branch divide`
        * create a branch named `divide`
    * `git checkout divide`
        * checks out the newly created `divide` branch




-
 Now you have a local branch, but you want to cover your bases and tell Github to start tracking a remote version of it.
 So, you run:
    * `git push -u origin divide`

-
 Now, a `git branch -a` shows:
> 
  master<br/>
  \* divide
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/add
  remotes/origin/subtract
  remotes/origin/multiply
  remotes/origin/divide
  
  
-
 Now you're ready to start developing.
* Modify a file called `Operations` and add your **divide** function.
* Also, modify `OperationsTest` and make unit tests for your new **divide** function.
* To get everything up to the remote server, do the following:
    * `git add .`
    * `git commit -m "Added divide function and unit tests"`
    * `git push`

-
* A `git status` should tell you that you're up to date.
* Now you're ready to merge your stuff into your master branch to make sure that everything's cool.


-
* `git pull` to make sure that you're up to date with the remote version.
* This ensures that you won't have issues with your merge if someone made a change to master.
* Next,`git checkout master` and `git merge divide`.
* If everything's good, then you will now have all of your commits on top of the master commits.  Otherwise, you'll have to handle a merge conflict.  We'll touch on this later.




-
-

## Git TL;DR
* `git clone {remote project}`
* `git branch {branchname}`
* `git checkout {branchname}`
* `git push -u origin {branchname}`
* Make changes to the project
* `git add .`
* `git commit -m "{commit messages}"`
* `git push`
* When you're ready to merge
* `git pull`
* `git checkout master`
* `git merge {branchname}`




-
-
## Merge Conflicts



-
 A merge conflict is what happens when you try to merge a changed file into another file that has been changed.

 Consider the following scenario:




-
* Matt Multiplication decided to implement the multiplication method himself.
* When you checkout master, the file you changed was in state A.
* When Matt decided to make the change, his version was also in state A.
* When Matt pushes his changes to master, the branch was in state

-

* You make your changes on your branch, but before you can merge it into your up-to-date version of master, Matt pushed his change.
* This means that the file is now in state B, and you're trying to put it in state B. You can't.
* Plus, considering that keep keeps a running log of the state of a file, you now need to make your changes as if they came after Matt's change (since it keeps the order).
* Congrats! Matt just helped usher you into the world of merge conflicts.

-
Now, I know merge conflicts are scary.  Keep in mind, though, that YOU CANNOT BREAK ANYTHING!  Git exists partially so that when people screw stuff up, you can just go back to a previous, safe version of the software.  So be strong.  We'll get through this.

-
Another thing to notice about merge conflicts is what happens to your conflicted files.  Notice the syntax of:
```
<<<<<<< Branch / HEAD
Some
changes
=======
Some different changes
>>>>>>> Other branch
```

-
There are a various ways to handle a merge conflict.  The easiest one, however, is to use `git mergetool` and use a tool to handle it.  This is hard to describe doing because each machine might use a different tool.  So, suggested tips for merge conflicts are the following:

-
##Suggested Tips for Merge Conflicts:
<p class="fragment fade-up">1. Commit early and commit often.  Your merge conflicts won't be all the large if you're constantly making small changes.</p>
<p class="fragment fade-up">2. Try and not work on the same files as other people.</p>
<p class="fragment fade-up">3. Be calm and Google things.  That's how most of us figured out how to fix merge conflicts.</p>

-
-
## Pull Requests

-
Since many repositories are protected, you'll need to submit a **PR** (pull request) to get your changes merged into them.  Think of it a a merge that someone has to approve. 

To submit from a **fork** (your personal copy of a repo that you can make changed to), go to your git provider (such as Github, Bitbucket, or a proprietary git server) and submit a **PR** from its interface.

A good practice is to get your **master** branch to your desired state, merge in the original project's changes, then submit your pull request. 

-

Each provider handles pull requests differently, so follow the directions that apply to you. However, remember they're basically protected merges, so treat them as you'd treat merges.

-
-

##Git Setup

-

We're going to use `git bash` for a few reasons:
* To actually learn the git commands
* To cover multiple systems and environments
* To teach you to be able to teach others

-
#Walkthrough

-
##Set up a Github account
https://www.github.com

-
##Fork this repo
https://git.zipcode.rocks/Cohort4.2/CR-Calcul8r.git

-
##Open the project in BlueJ
* Open the project from the file system
* Show the dotfiles

-
##Add your divide function

-
##Commit and push it

-
##Make your PR

-
##We'll cover resolving merge conflicts later.

-
<img src="/tcuk-slides-light/img/bunnies/rabbits-huddling-together.jpg" >



