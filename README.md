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

**You made an mistake didn't you? (If not then I made one :D)**

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

Keep in mind that this might help you in an emergency (google for more details)
```
git rebase --abort
```
