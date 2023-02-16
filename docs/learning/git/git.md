---
title: Version Control with Git
---

This is used as a reference sheet for people starting out in CS and want a crash course on Git. 

## The Problem
Let's first address why we use git. Think about the following situation:

1. You just created a new project to manage. There
2. Your friend wants to collaborate on coe i

## Intro to Version Control


### Branching

### Merging

### Rebasing

#### Rebasing when your local branch is out of date
```
gitGraph
    commit
```
### Other Git Tools

## Remote Version Control
Now that you are able to control the versioning of your project, how do you collaborate on it with other people? Git only managed the versioning on your local machine so we need to bring in a new tool: **Github**.

Github allows you to create a repository that exists out in the internet called the `remote repository`. You can store your own code and its different versions on github and allow other people to access them.

### Setting up your Remote Repo

On your local machine:
``` bash
git remote add origin <remote-url>
```
- create a git repository by creating a folder and running `git init`. This will generate a `.git` folder (any folder with a `.git` folder in it is consiered a repository)
- `git remote add` tells github that you want to link your local repository with a remote one
- `origin <remote-urL>` is the alias for your remote repository and the link associated with it. In the future when you want to reference the remote, use `origin`. Technically you can choose any other word as this alias, but `origin` is the convention. 

You are now ready to track changes in your repository!

### Pushing

### Pulling

### Collaborating on Remote Codebases

## Sample Git Workflow
