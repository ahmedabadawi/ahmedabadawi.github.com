---
layout: post
title: Awesomeness of Linux - Display Directory Structure - 'tree' Command
categories: Linux
---

I was working on my last post and found out that I used a Linux command **tree** to take a screenshot of the directory structure, then I thought, Oh that was kinda cool. So I thought of writing a small post about it.

## tree - Command
The tree command is a simple utility to visualize a directory structure and has few useful options.
![Directory Structure](/assets/docker-microservices-dev-env-directory-structure.png)
- Display all files including hidden files
```
tree -a
```
- Display only directories (the one used in the screenshot above)
```
tree -d
```
- Limit the tree level (also used in the screenshot above)
```
tree -L 2
```
- Colorize the output
```
tree -C
```
- Similar command 'pstree' to view the running processes in a tree
```
pstree [pid|username] # pid and username are optional, when omitted it displays all processes
```
