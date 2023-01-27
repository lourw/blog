---
title: "Part 1: REPL"
description: Read-eval-print-loop that controls the database CLI
---

## Overview
The first step in implementing a database is to set up the user interface that allows a user to interact with the underlying database once it's built. Using a simple while loop and some IO commands, this part sets up a simple while loop CLI that waits for user input and completes an action based on a given input. 

## Challenges
For this first part, the core logic being implemented was mostly a while loop and some IO features. The REPL just needed to complete the following actions: 

1. await user input when run
2. read and process user input
3. output some result or text back to user

There wasn't much complexity in this step other than figuring out rust's IO libraries. In C you have the `string.h` library that handles a lot of the string manipulation and `stdio.h` for managing IO. 

