+++
title = 'Git Stuff'
date = 2011-07-08T13:08:46+02:00
draft = true
+++

# Git Stuff

```
This has been originally posted ages ago in an old private blog of mine and has been imported here.
I might not be proud about some of the formulations and use of swear words and slurs but to keep this
true to the original post, I have not modified anything. Don't blame me, blame past Martin. :)
```

Since working at Nokia, I have the pleasure to work with a ‘Distributed Version Control System’. As I have used mostly perforce before, the switch was both, a blessing and a curse.

I have to admit, that I had massive problems to get used to it in the beginning. But by now, git and me are BFFs … at least until random shit starts to happen again :).
Yeah, I know that this is mine fault and not git’s. It is just so damn easy to do something wrong. Git is far away from a submit-and-run VCS like perforce, but that is a fair price for the fact, that you can now branch whenever you want without days of integration pain.

I do not want to go into to much details here, as there are more than enough very good tutorials out there. If you are new to DVCS, check out [Joel’s brilliant article](https://www.joelonsoftware.com/2010/03/17/distributed-version-control-is-here-to-stay-baby/).

Here are some (hopefully) useful tips for working with git.

<!--more-->

## Git in Dropbox

That one is pretty obvious, but extremely useful, especially for private projects. You can push your local repo to your Dropbox and it automatically gets synced with all the PCs you are using Dropbox with.

```bash
# go to your Dropbox and create your project directory

$ cd ~/Dropbox
$ mkdir my_project
$ cd my_project

# now initialize your git repo with
$ git --bare init

# As you have your remote-repo prepared, go to your local repository.
$ cd ~/dev/my_project

# First, you need to introduce the remote location to git
# this adds the specified path as the remote named 'origin'
# but you could as well name it 'Dropbox' or 'whatever'
$ git remote add origin file:///home/user/Dropbox/git/my_project

# git is set up, so push it to the remote ( 'origin' or whatever
# name you have used ).
$ git push origin master
```

Done, you know have your repo on your Dropbox. If you are on another PC and want to
access it, just clone it from there, and you are set. You can use this like you would
use any git-server.

## Save the history, with rebase

As your local repo is basically a branch of the remote repo, the default behavior of git pull is a merge. There is nothing really wrong about this, but if you work on larger projects with lots of contributors, this makes your history really hard to read.

You can avoid this quite easily by using rebase instead:

```bash
git pull --rebase
```

The main difference is the way the merge happens. With rebase, your commits are ‘removed’, the remote changes are applied and after that your changes are applied on top of the remote changes. This preserves a linear history and makes it human readable again.

### Interactive Rebase FTW!

The interactive rebase allows you to modify already committed changes. Let’s say you are prototyping something. Instead of waiting for a good state to commit your changes, you can commit as often as you want. When you are ready to push, you can do the interactive rebase and put commits together, remove them completely or change the commit messages.
So, you have been prototyping a feature and realized that you need to refactor a bit of old code in this process. Let’s assume you have now 5 small checkins. 2 changes are small refactoring and the other 3 are iterations of the feature you are prototyping. You realize that it would make more sense to have only 2 commits. One for the refactoring, and one for your feature.

```bash
# you need to tell interactive rebase in which commits you are interested in
# ( in our case these are the last 5 commits )
$ git rebase -i HEAD~5
```

This will put you into the rebase mode, where you can select what you want to do with these changes.

```bash
pick 5c6bb74 some refactoring
pick 91dbdfa other refactoring
pick 3080d61 iteration 1
pick 4e4f56a iteration 2
pick 1890f70 iteration 3

# Rebase a37f00c..1890f70 onto a37f00c
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.
#
```

You can now alter the changes. In this case we want to group them and change their
commit messages. The result could look like this:

```bash
reword 5c6bb74 some refactoring # changes the commit message
fixup 91dbdfa other refactoring # groups this commit with the previous
reword 3080d61 iteration 1 # changes the commit message
fixup 4e4f56a iteration 2 # groups this commit with the previous
fixup 1890f70 iteration 3 # groups this commit with the previous
```

After you have done this, you will be prompted for the commit messages of the two rewords. When finished, you have only two commits left and they have the proper change description. You can now push this without having a bad conscience. This is how the history now looks like:

```bash
$ git log
commit 70f40f9504e5721c7bce32fe9a8c792cddce6acf
Author: Martin Zielinski
Date: Thu Jul 7 23:50:14 2011 +0200

    feature xyz

commit 4e47d572508b1109097f73959fe7be02e23ee437
Author: Martin Zielinski
Date: Thu Jul 7 23:49:22 2011 +0200

    refactoring old code
```
