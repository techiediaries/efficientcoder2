---
layout: post
title: "How to Delete Local/Remote Git Branches" 
date: 2020-09-05
tags: [git, python]
excerpt: "If you have previously worked with Git for versioning your Angular code, there is a good chance that you had some situation where you wanted to delete a remote branch or multiple branches. This happens many times to developers, particularly in large projects"
---

If you have previously worked with Git for versioning your Angular code, there is a good chance that you had some situation where you wanted to delete a remote branch or multiple branches. This happens many times to developers, particularly in large projects.

In this article, we'll learn:

- How to delete a local branch in your Git repository,
- How to delete a remote branch in Git,
- How to delete all Git branches which have been merged,
- How to remove all local branches not on remote,
- How to delete all your local Git branches except master.


Before tackling how to delete a remote branch, we’ll first see how to delete a branch in the local Git repository.

> **Note:** Version control systems are an indispensable tool in modern web development that can help you solve many issues related to every task. Git is one of the most popular version control systems nowadays.

Before we proceed to learn how to delete local and remote branches in Git, let's define what's a Git branch and the side effects of deleting branches.

A branch in Git is a pointer to a commit. If you delete a branch, it  deletes the pointer to the commit. This means if you delete a branch which is not yet merged and the commits become unreachable by any other branch or tag, the Git garbage collection will eventually remove the unreachable commits.


## Deleting Local Branches

Let's start by learning how to delete a local branch.

1.  First, use the `git branch -a`  command to display all branches (both local and remote).
2. Next, you can delete the local branch, using the  `git branch -d`  command, followed by the name of the branch you want to delete.

```bash
$ git branch -a 

# *master # b1 # remote/origin/master # remote/origin/b1 
$ git branch -d b1 # Deleted branch b1.
```

>**Note:** You can also use the `-D` flag which is equivalent to the `--delete --force` command instead of `-d`. This will enable you to delete the local branch regardless of its merge status.

## Deleting Remote Branches

Unlike local branches, you can't delete a remote branch using the  `git branch`  command. However, you need to use the  `git push --delete`  command, followed by the name of the branch you want to delete. You also need to specify the  `remote`  name (`origin`  in this case) after  `git push`.

```bash
$ git branch -a

# *master
# b1
# remote/origin/master
# remote/origin/b1

$ git push origin --delete b1
# [...]
# - [deleted] b1
```

## How Can You Delete All Non-Merged Git Branches?

Now that we have seen how can you [delete local and remote branches](https://www.techiediaries.com/delete-local-remote-git-branches/) in your Git repositories, let's suppose you have multiple Git branches. How can you delete the branches which have already been merged? At once instead of deleting them branch by branch.

> **Note**: Merging is performed using the `git merge` command and it simply means integrating changes from another branch.

 
First, you need to get all the branches that are merged in the remote repository using the following command:

```bash
$ git branch --merged
```

If you have one merged branch, you can simply delete the merged local branch using the following command:

```bash
$ git branch -d branch-name
```

If you want to delete it from the remote repository use the following command:

```bash
$ git push --delete origin branch-name
```

### Remove All Local Branches not on Remote

You can remove all local branches not on the remote repository, you can use the following bash command:

```bash
$ git branch -r | egrep -v -f /dev/fd/0  <(git branch -vv | grep origin) | xargs git branch -d
```

Let's break this command:

1.  First we get all remote branches using the `git branch -r`command
2.  Next, we get the  local branches not on the remote using the`egrep -v -f /dev/fd/0  <(git branch -vv | grep origin)` command,      
3.  Finally we delete the branches using the `xargs git branch -d` command.

> [grep](https://en.wikipedia.org/wiki/Grep) is a command-line utility for searching plain-text data sets for lines that match a regular expression. Its name comes from the ed command g/re/p (globally search for a regular expression and print matching lines), which has the same effect. grep was originally developed for the Unix operating system, but later available for all Unix-like systems and some others such as OS-9.

> [xargs](https://en.wikipedia.org/wiki/Xargs) (short for "eXtended ARGuments") is a command on Unix and most Unix-like operating systems used to build and execute commands from standard input. It converts input from standard input into arguments to a command.



### Delete All Your Local Git Branches Except Master

If you have finished with local Git branches, it's usually a good practice to remove them to free their space. You can simply run the following command:
 
```bash
$ git branch | grep -v "master" | xargs git branch -D 
```

We use the `grep -v "master"` command to search for branches except the master then we delete them using the `git branch -D` command.


## Conclusion

Throughout this article, we've seen how you can delete remote and local branches from your Git repositories. We've learned:

- How to delete a local branch in your Git repository,
- How to delete a remote branch in Git,
- How to delete all Git branches which have been merged,
- How to remove all local branches not on remote,
- How to delete all your local Git branches except master.
