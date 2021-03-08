---
title: "Intro to git"
author: "Chris Oldmeadow"
institute: "HMRI, CReDITSS"
date: "2020/08/17"
---
## Why <i class="fas fa-git"></i>

<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.6.1/css/all.css" integrity="sha384-gfdkjb5BdAXd+lj+gudLWI+BXq4IuLW5IT+brZEZsLFm++aCMlF1V92rMkPaX4PP" crossorigin="anonymous">

![phd](phd101212s.gif){style="border: 0; background: none; height: 600px; vertical-align: middle" }


## What is <i class="fas fa-git"></i>
![Who has done this before?](https://imgs.xkcd.com/comics/git.png)

### Tracks changes to files and directories



### Contents of directory represented by Trees  (and blobs)

* Tree = folder
* Blob = file

### Git represents history/changes to trees through a DAG

![A Directed Acyclic Graph](basic-merging-2.png)

[]()


## Key concepts

* Repository
* Working directory
* Staging files
* Commits 
* Branches
* HEAD 

### Repository

* contains all the files that are being tracked (as well as the necessary
  database to facilitate this)
* can be local or remote (eg on GitHub)

* command: git init
* this initialises a local repository

### Working Directors

* contains all the files and folders for your project
* you may want to track some or all of these files 
* they are not tracked until they are:
    * Staged
    * Committed

### Staging files

Rather than committing everything, we specify which files to commit.

* command: git add filename

### Commit changes

* A snapshot of the changes you have made that you want to track
* Should have a detailed message explaining what was different

* command: git commit -m "My short message" 


### Branches
These are pointers to specific commit histories

### HEAD

This is a pointer to the current view of the repository (the last snapshop
excluding staged changes)

##   A simple example

### Creating a local repository
<div style="color: #42affa">
```console
❯ git init
Initialized empty Git repository in /home/chris/git-test/.git/
```
</div>

### Create some files for the repo

I have added the file *example.R*

```r
y <- rnorm(100)
hist(y)
```
### Now lets view the status of the repo

<div style="color: #42affa">
```console
❯ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        example.R

nothing added to commit but untracked files present (use "git add" to track)
```
</div>


### Add a file/s to the staging area
<div style="color: #42affa">
```console
❯ git add example.R
❯ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   example.R
```
</div>


### Committing changes to the repository
<div style="color: #42affa">
```console
❯ git commit -m "Initial commit"
[master (root-commit) deb6e03] Initial commit
 1 file changed, 2 insertions(+)
 create mode 100644 example.R
```
</div>

<div style="color: #42affa">
```console
❯ git status
On branch master
nothing to commit, working tree clean
```
</div>
### View the log of the repo commits
<div style="color: #42affa">
```console
❯ git log
commit deb6e0338091fe67d93d8a4e9945eed2c6cde448 (HEAD -> master)
Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
Date:   Sun Feb 21 13:24:16 2021 +1100

    Initial commit
```
</div>


### Making some edits to the file
<div style="color: #42affa">
```r
y <- rnorm(100)
hist(y)
x <- rbinom(100,1,0.5)
summary(lm(y ~ x))
```
</div>

### View the status of the repo
<div style="color: #42affa">
```console
❯ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   example.R

no changes added to commit (use "git add" and/or "git commit -a")
```
</div>

### Add and Commit the modification and view the log
<div style="color: #42affa">
```console
❯ git add example.R
❯ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   example.R
❯ git commit -m "Added an independent varibale and a regression"
[master 6339b50] Added an independent variable and a regression
 1 file changed, 2 insertions(+)
❯ git log
commit 6339b50aa3ccbe6c21ffdda2fdcdf1ca90ef9791 (HEAD -> master)
Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
Date:   Sun Feb 21 13:38:46 2021 +1100

    Added an independent variable and a regression

commit deb6e0338091fe67d93d8a4e9945eed2c6cde448
Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
Date:   Sun Feb 21 13:24:16 2021 +1100

    Initial commit
```
</div>



### Resetting back to the previous version
<div style="color: #42affa">
```sh
❯ git reset --hard HEAD~1
```
</div>

### What does the repo look like now?
<div style="color: #42affa">
```console
❯ git status
On branch master
nothing to commit, working tree clean
❯ cat example.R
y <- rnorm(100)
hist(y)
```
</div>

### Alternatively just reset the commit
<div style="color: #42affa">
```sh
❯ git reset HEAD~1
Unstaged changes after reset:
M       example.R
```
</div>
The edits are still in the file, but not yet committed

## Branches (optional)
<div style="color: #42affa">
```sh
❯ git branch experimental
❯ git switch experimental
```
</div>

### Add some content to the experimental branch
```r
y <- rnorm(100)
hist(y)
x <- rbinom(100,1,0.5)
summary(lm(y ~ x))
z <- rbinom(100, 1, 0.7)
summary(lm(y ~ x + z))
```

### View the status of the new branch
<div style="color: #42affa">
```console
❯ git status
On branch experimental
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   example.R

no changes added to commit (use "git add" and/or "git commit -a")
❯ git add example.R
❯ git status
On branch experimental
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   example.R
```
</div>

### Commit the changes and view the log
<div style="color: #42affa">
```console
❯ git commit -m "Controlling for a potential confounder"
❯ git log --graph --abbrev-commit --decorate --all
* commit 9e444b9 (HEAD -> experimental)
| Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
| Date:   Sun Feb 21 14:04:01 2021 +1100
|
|     Controlling for a potential confounder
|
* commit f555026 (master)
| Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
| Date:   Sun Feb 21 13:48:17 2021 +1100
|
|     Added independent variable and a regression
|
* commit deb6e03
  Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
  Date:   Sun Feb 21 13:24:16 2021 +1100

      Initial commit

```
</div>

### Return to the master branch
<div style="color: #42affa">
```console
❯ git switch master
❯ git log --graph --abbrev-commit --decorate --all
* commit 9e444b9 (experimental)
| Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
| Date:   Sun Feb 21 14:04:01 2021 +1100
|
|     Controlling for a potential confounder
|
* commit f555026 (HEAD -> master)
| Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
| Date:   Sun Feb 21 13:48:17 2021 +1100
|
|     Added independent variable and a regression
|
* commit deb6e03
  Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
  Date:   Sun Feb 21 13:24:16 2021 +1100

      Initial commit
```
</div>


### Make some changes on this branch

```r
y <- rnorm(100)
hist(y)
x <- rbinom(100,1,0.5)
summary(lm(sqrt(y) ~ x ))
```

### Add and commit the changes

<div style="color: #42affa">
```console
> git add example.R
> git commit m "Transformed the response"
> git log --graph --abbrev-commit --decorate --all 
* commit 530e65a (HEAD -> master)
| Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
| Date:   Sun Feb 21 14:20:06 2021 +1100
|
|     Transformed the response
|
| * commit 9e444b9 (experimental)
|/  Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
|   Date:   Sun Feb 21 14:04:01 2021 +1100
|
|       Controlling for a potential confounder
|
* commit f555026
| Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
| Date:   Sun Feb 21 13:48:17 2021 +1100
|
|     Added independent variable and a regression
|
* commit deb6e03
  Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
  Date:   Sun Feb 21 13:24:16 2021 +1100

      Initial commit
```
</div>


### Now lets merge master with experimental
<div style="color: #42affa">
```console
❯ git merge experimental

```
</div>
Git will attempt to merge the files, but here it is not smart enough, and we
will get a *merge conflict*, manually edit the file to fix the conflicts

### Add and commit the changes
<div style="color: #42affa">
```console
❯ git add example.R
> git commit -m "Merged with experimental"
> git log
*   commit 73a1af0 (HEAD -> master)
|\  Merge: 530e65a 9e444b9
| | Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
| | Date:   Sun Feb 21 14:30:07 2021 +1100
| |
| |     Merged with experimental
| |
| * commit 9e444b9 (experimental)
| | Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
| | Date:   Sun Feb 21 14:04:01 2021 +1100
| |
| |     Controlling for a potential confounder
| |
* | commit 530e65a
|/  Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
|   Date:   Sun Feb 21 14:20:06 2021 +1100
|
|       Transformed the response
|
* commit f555026
| Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
| Date:   Sun Feb 21 13:48:17 2021 +1100
|
|     Added independent variable and a regression
|
* commit deb6e03
  Author: Chris Oldmeadow <chris.oldmeadow@protonmail.com>
  Date:   Sun Feb 21 13:24:16 2021 +1100

      Initial commit
```
</div>



## Remote repositories

### Gittea
We have a private Gittea server at [https://git.hmri.org.au](https://git.hmri.org.au)
<div style="color: #42affa">
```sh
$ git remote add origin https://git.hmri.org.au/username/repo
```
</div>


### Pushing to an existing remote repo

<div style="color: #42affa">
```sh
$ git push origin master
```
</div>




### Pulling changes from a repo

<div style="color: #42affa">
```sh
$ git pull origin master
```
</div>




### Cloning a repo


<div style="color: #42affa">
```sh
$ git clone https://git.hmri.org.au/coldmeadow/project_template
```

* rename the project directory
* remove the .git folder
* initialise the repo

</div>
