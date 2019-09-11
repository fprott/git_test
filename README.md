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
git push
#OR 
git commit -a -m "Your Message"
```

**To get something**

```
#This will download stuff from REMOTE and apply it!
git pull
```
This picture shows how this works:
https://i.stack.imgur.com/nWYnQ.png
