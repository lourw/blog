---
title: Git Hooks
---

You can add automation to your git workflow. 

1. Add a file into the ./git/hooks directory of your repo
2. Add a new file with a script corresonding for your hook. The file name should be one of:
    * pre-commit
    * pre-merge
    * pre-push
    * pre-rebase
    * pre-applypatch
    * pre-receive
    * ...there are more as well
3. Insert your custom script into the new file
4. Set the file as executable with 
    ```
    chmod +x pre-commit
    ```

## References
* [Atlassian Docs](https://www.atlassian.com/git/tutorials/git-hooks)
    
