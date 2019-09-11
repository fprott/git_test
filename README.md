
### I use this old repo to explain stuff, first the fact that the README.md in GitHub is always coded with the Markdown syntax

It's very easy to make some words **bold** and other words *italic* with Markdown. You can even [link to the Tutorial that explain all!](https://guides.github.com/features/mastering-markdown/)

But for more complex task try using the wiki with mediawiki syntax!

So lets start
------------
*First explain the basics using this link https://stackoverflow.com/questions/8198105/how-does-git-store-files*

**Then config your stuff**
```
git config --global user.name "Your name here"
git config --global user.email "your_email@example.com"
#If you work on linux you may want to enable pretty colors
git config --global color.ui true
#If in Linux you also really should change the editor, else you may use vim
git config --global core.editor nano
```

Now we need to create a ssh key which is basically our password replacement. That thing is super important, and you can have many (each per computer is recommended) or just one.

Under the Windows cmd or Linux you can:

```
ssh-keygen -t rsa -C "your_email@example.com"

```

Under linux you now find a hidden folder in your home called *.ssh* . Under Windows it is in C:\Users\<Benutzername>\.ssh\ and possible hidden. **THIS is super important, do not loose or expose this folder to anyone**

In that folders are two files a id_rsa.pub and a id_rsa file.
**ONLY EVER use .pub files!!! The other files contain your private key! Never give them away!**
Copy the contents of the .pub file and paste them into GitHub via:
* Go to your github Account Settings
* Click “SSH Keys” on the left.
* Click “Add SSH Key” on the right.
* Add a label (like “My laptop”) and paste the public key into the big text box.

For Windows this small tutorial might help:
https://dhue.de/git-fur-windows-installieren-und-ssh-keys-nutzen/ 


**Now get really started**

```
#this will make an empty repo, try it now
git init 
#if you want to clone this then use
git clone git@github.com:fprott/git_test.git
#be ware, if you operate behind a firewall or do not want to configure the computer with your certificate and use a private repo then http is better !
git clone https://github.com/fprott/git_test.git
```
To add something
------------
```
#To add stuff
git add *
git commit -m "Your Message"
git status
#Now check if everything is alright
git push
#OR 
#Note, you still need to add new Files
git add NEW_FILE 
git commit -a -m "Your Message"
```

To get something
------------
```
#This will download stuff from REMOTE and apply it!
git pull
```
This picture shows how this works:
https://i.stack.imgur.com/nWYnQ.png

**Now two really useful commands**
```
#what is going on
git status
#what changed
git diff
#I really need a GUI to see what happens (NOT RECOMMENDED if GitKraken or GitDesktop is installed)
gitk
```

**You made an mistake didn't you? (If not then make one :D)**

Mistake **before** you committed?
```
git reset --hard HEAD
git checkout HEAD
```
Mistake **after** you committed?
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
*Explain how and why to use what GUI Program*

Now let's branch!
------------
Work on a branch for all and every experimental features, let the master be always buildable if possible. On project where this is not possible we will instead use *tags* to mark the buildable versions!
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

**In case you have locale changes you want to keep but not commit**
It is possible to save your locale changes in a "array of quick-saves" before checking out other stuff or even working on other stuff. This is called stash. You can use it like this
```
#saves your local stuff but does not apply it to the repo directly
git stash
#Now you can do whatever
#if you want your stuff back use
git apply
#and if you want your stuff back and clear the stash
git pop
```
Keep in mind that your stash is an stack (a list of objects with first in first out policy). So you can stash multiple times!

To save your work
------------
You know how to add stuff but this is not all! You also need to send that stuff away to the remote repo. This is a **MUST and should be done daily** (trust me I was the worst at doing that :D)

To do so we push **after** we committed.
```
git push origin master
#if you have just one remote repo and only the master
git push 
#if you want to push to a specific branch
git push origin BRANCHNAME
#in case you have many branches
git push --all origin
#Note you can have more then one remot repo! This is advanced and is not handled here but you can simply change the name
git push other_remot master
```

Ok, lets scew something really up for a change :D
------------
We want to show you can handle the worst problem: merge-conflicts. This happens if a file is changed in a way that it is not logicaly possible to prove which vesion is better

```
#We make a new branch and check it out
git checkout -b error_branch
#make an edit in the pi file
git checkout master
#make a different edit in the pi file
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

*\=\=\=\=\=\=\=* is a separator, it has no function but to separate the merges in the order you merged them

*regular code* is just code.


**What you need to do is to delete all stuff between the <<<<<<< and >>>>>>> Markers that is not wanted! This does include the =======! You can modify the code how you want but you must delete everything but code!** 
**In the end you need to have compilable code!**


Done? Good then save and do
```
#add everything
git add .
#you do not need a commit comment since there will be one generated for you. But you can give one if you like!
git commit 
```

*Alternatively you may also use the easy console way by doing this* **instead**

```
#GUIs can do this way better but there is a interactive way
git merge -i 
```

**Ok, now I want to show a special kind of branch called the *octopus* problem. This is where GitKraken got his name and it is a hard problem for many version control software. But don't worry I only show the basics.**

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

**If you have a merge conflict do multiple separate merges first !**

**Use GitKraken to do complex operations with conflicts or merge!**

**If you work in a team try to either ignore contested files (configs usually) or have a separate merge strategies for option files!**

**Alternatively use submodules!**

Help, I lost my head on the way
------------
Okay, that can happen (and does happen when you either work with submodules or rebase) do not worry.

*What happened?*
Well you now are in a part of the tree that is not the end of a branch. And therefore you are currently merely visiting an old state. 
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

When to force?
------------

First of all, forcing is almost **never a good idea!** Especially **Never** do this: *git push --force* ! Use *git push --force-with-lease* instead.

What you may want to do sometimes is to pull with force. That way you will **overwrite** your locale stuff! But the standard *git pull -f* is still a bad idea. Use the code below instead. Keep in mind that everything not on the remote repo **can be lost**!!!
```
#we fetch (this is like pulling but without applying the changes
git fetch --all
git reset --hard origin/master
#can be done with any branch
```

Sometimes you really want to remove a branch. Like when you were really on the wrong track and are ashamed of your work. We all have been there! *Keep in mind that the changes are still possibly retrievable (at least for a short time) in case you pushed sensitive data like passwords!**
```
git branch -D BRACH_NAME
```

Git ignore and large files
------------

You want to have a file called *.gitignore* in your folder. This file will be hidden in Linux after you made it (since it carries a postfix point). That file allows you to ignore certain files. This is needed for
* all your IDE specific files and folders
* large files
* temporary files
* lokal files (like databases)
* certifcates
* everything that needs to be secure

To make something ignored simply add to the file:
```
# A comment
# Name of a specific file (including the extension)
filename.type 
anyfile.txt
# if you want to blacklist all html files
*.html
#if you want to exclude a specific html file
!index.html
#if you want to blacklist .o and .i files
*.[oi]
#if you want to blacklist all files named generic (with many extensions)
*.generic
#if you want to exclude the Python folder (for Python cache)
.Python
```
All files that excluded will not be added when you type *add **

**Do this to large files since your repo can handle large files but GitHub does NOT**

*What if I need large files?*
Simple, there is an extension for them! Use the following two tutorials
https://git-lfs.github.com/
https://help.github.com/en/articles/configuring-git-large-file-storage

How to deploy?
------------

Normally we want to have a good way to deploy the software onto the beta-testers and applications as soon as possible.
**Since this is a basic tutorial I will only tell what it does, not how to. This is about awareness*
There are two common ways to do it (and the manual way which is also too common):
* Deamon
* Webhooks

A *Deamon* usually takes the stuff that is either pushed or pull requested onto a special branch or under a special tag, tests it (test pipeline) and then pushes it onto the field repos. It may also run code to directly compile and/or modify the code arcoding to the device. So a old computer gets the instructions to use Algo A instead of the better but more costly algo B.
*We do not handle Deamons her since we only have 5 Git users and we need them all!*

** Webhooks **

*show where the webhook API in GitHub is ( https://github.com/ORGA/REPO/settings/keys )*
Apply the ssh keys for your web-hook! After that you can send a JSON or x-www-form (none standardized) to your application with
https://github.com/ORGA/REPO/settings/hooks/new

You can either push everything or have specific events. This is nice for scripts since they can run without compiling (NOTE: Python has to restart)

GitHub will now send you an request and/or *pull for you* 

**Keep in mind you should only do this under the ssh key protection **

