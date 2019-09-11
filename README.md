### I use this old repo to explain stuff, first the fact that the README.md in GitHub is always coded with the Markdown syntax

It's very easy to make some words **bold** and other words *italic* with Markdown. You can even [link to the Tutorial that explain all!](https://guides.github.com/features/mastering-markdown/)

But for more complex task try using the wiki with mediawiki syntax!

**So lets start**

*First explain the basics using this link https://stackoverflow.com/questions/8198105/how-does-git-store-files*

```
#this will make an empty repo, try it now
git init 
#if you want to clone this then use
git clone git@github.com:fprott/git_test.git
#be ware, if you operate behind a firewall or do not want to configure the computer with your certificate and use a private repo then http is better !
git clone https://github.com/fprott/git_test.git
```
**To add something**

```
#To add stuff
git add *
git commit -m "Your Message"
git status
#Now check if everthing is allright
git push
#OR 
#Note, you still need to add new Files
git add NEW_FILE 
git commit -a -m "Your Message"
```

**To get something**

```
#This will download stuff from REMOTE and apply it!
git pull
```
This picture shows how this works:
https://i.stack.imgur.com/nWYnQ.png

**You made an mistake didn't you? (If not then make one :D)**

Mistake **before** you commited?
```
git reset --hard HEAD
git checkout HEAD
```
Mistake **after** you commited?
```
#Use ^ for later Head!
git revert HEAD
#WARNING, you will now meet your text editor. In case it is VIM -> enter the WORD ":q" to leave it!
```
Mistake made in the **commit comment**
```
git commit --amend
```
You forgot to add a file:
```
git add missed-file.txt
git commit --amend
```
You added something that you really should not:
```
git reset wrong-file
```

_**Why do we do this the old fashioned way?**_
**Because every (!) GUI is using these commands, to understand git you need to understand these commands!**
*Explain how and why to use what GUI Programms*

**Now let's branch!**
Work on a branch for all and every experimental fetatures, let the master be always buildable if possible. On project where this is not possible we will instead use *tags* to mark the buildable versions!
```
#Make a branch called A
branch A 
checkout A
#You see the branches with
branch
```
Now change something and merge to the master
```
#Now change something in the branch but NOT in the master
#Now we switch in the branch we want to "keep" / be the one *merged too*
git checkout master
git merge BRANCHNAME
```

Ok, if you want to combine the branch with the master **without** a merge to keep it clean
```
#Now change something in the branch but NOT in the master
#If you are not in the branch check it out 
git checkout BRANCHNAME
git rebase master
#Now we ahead of our own master who is way behind us!
#We have to merge it to ourselfe
git checkout master
git merge BRANCHNAME
#Now you may want to delte your branch, but this is only needed if you really need a clean look. Usually don't do this !
git branch -d BRANCHNAME
```
Note, when you rebase or use submodules you may loose your head. If that happens then checkout out [the instructions future down](#Help, I lost my head on the way).

Keep in mind that this might help you in an emergency (google for more details)
```
git rebase --abort
```

Also if you made an spelling error in the branch name
```
git branch -m feature-brunch feature-branch
```

*Ok, lets scew something really up for a change :D*
We want to show you can handel the worst problem: merge-conflicts. This happens if a file is changed in a way that it is not logicaly possible to prove which vesion is better

```
#We make a new branch and check it out
git checkout -b error_branch
#make an edit in the pi file
git checkout master
#make a diffrent edit in the pi file
#Now we have a problem :D
```
There a two ways to fix this without a GUI! However GUIs excel at merge conflicts, you should normally do this always by GUI.

First do it the normal way *This can happen even with GUI tools. Eclipse VC is notorious for this*
```
git merge error_branch
#Note that this will fail and give you a warning
#We now have to fix this by hand. Open the problem file with a *text editor*, I use nano (in windows please use notepad++ trough the explorer)
nano pi.txt #On windows do this by clicking on the file
```

Ok, you now see four types of things:

*<<<<<<< HEAD* is the branch you merge too

*>>>>>>> error_branch* is one of the branches you merge from

*\=\=\=\=\=\=\=* is a seperator, it has no function but to seperate the merges in the order you merged them

*regular code* is just code.


**What you need to do is to delete all stuff betewen the <<<<<<< and >>>>>>> Markers that is not wanted! This does include the =======! You can modify the code how you want but you must delete everything but code!** 
**In the end you need to have compilable code!**


Done? Good then save and do
```
#add everthing
git add .
#you do not need a commit comment since there will be one generated for you. But you can give one if you like!
git commit 
```

*Alternativ you may also use the easy console way by doing this* **instead**

```
#GUIs can do this way better but there is a interactive way
git merge -i 
```

**Ok, now I want to show a special kind of branch called the *octopus* problem. This is where GitKraken got his name and it is a hard problem for many version controll software. But don't worry I only show the basics.**

```
#assume or make at least tree branches, note the master is also a branch but we do not use it for now
git checkout -b A
#add file A
git add *
git commit -m "added A for testing reasons"
#go to the past
git checkout master
git checkout -b B
#add file B
git add *
git commit -a -m "added B for testing reasons"
git checkout master
git checkout -b C
#add file C
git add *
git commit -a -m "added C for testing reasons"
git checkout master
git checkout A
#NOTE we seperate by space not with a , or ; 
git merge B C
```

**NOTE: Octopus merges are very very hard to resolve if you have merge conflicts! I strongly recommend NOT doing them on conflicted code! It is not meant to do this!**

**If you have a merge conflict do multiple seperate merges first !**

**Use GitKraken to do complex operations with conflicts or merge!**

**If you work in a team try to either ignore contested files (configs usually) or have a seperate merge strategie for option files!**

**Alternativly use submodules!**

Help, I lost my head on the way
------------
Okay, that can happen (and does happen when you either work with submodules or rebase) do not worry.

*What happend?*
Well you now are in a part of the tree that is not the end of a branch. And therfore you are currently merly visting an old state. 
*That means?*
Every change you make will **not** belong to any branch!
*So I should no change anything?*
Correct. **Do NOT change anything!** Instead try to either fix this by getting back to your HEAD or branch out!

This will bring you back to the head, but **beware** this is not always the right state (check the code!):
```
git checkout HEAD
````

In case you work with submoduls you often just are not on the right master/branch so just use:
```
git checkout master
```

If you want to work on none HEAD state then
```
#Find out where you are
git reflog
#check out the right point or stay where you are
git checkout SHA
#then branch to make a new head
git branch NAME
```
