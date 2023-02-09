---
title: Shell Configuration
---

This is my default setup whenever I initiate a new shell on a computer

## Prompt
You can change how your terminal prompt looks. 

``` bash
lourw:02/08/23:~/Projects/development [main] ~ echo "Hello"
```

To achieve this, put the following in your .bashrc/.zshrc

=== "Zsh"
	``` bash
	function parse_git_branch() {
		git branch 2> /dev/null | sed -n -e 's/^\* \(.*\)/[\1]/p'
	}
	setopt PROMPT_SUBST
	export PROMPT='%F{white}%n:%W%f%F{green} %~ $%f%F{cyan}(parse_git_branch)%f%F{green}'$'\n''~%f '
	```
=== "Bash"
	``` bash
	function parse_git_branch() {
     		git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
	}
	export PS1="\u@\h \[\e[32m\]\w \[\e[91m\]\$(parse_git_branch)\[\e[00m\]$ "
	```

## References
* [Sample Bash Prompt](https://thucnc.medium.com/how-to-show-current-git-branch-with-colors-in-bash-prompt-380d05a24745)
