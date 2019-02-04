---
layout: post_default
title:  "Git branch in your prompt"
date:   2018-01-17 20:41:00 +4:00:00
categories: ["blog", "programming"]
author: "alicja"
published: true
---
I have paired a lot over the past week (which makes me tremendously happy!)
and I noticed how lost I can get in someone else's terminal if I don't have
my regular bash prompt - not only the cute little heart that welcomes me
every time I run the terminal, but also the git branch name I'm currently on.

As I couldn't find the tutorial I used to learn how to parse the git branch, 
I decided to go through my `.bashrc` and write one myself.

### Bash prompt variables
Bash prompt variables hold the text that shows as the terminal prompt in 
various contexts. Their values are examined and executed just before Bash 
prints them. There are several of them, but for now I'm going to 
focus on `PS1`, which is the primary prompt string.

In a bash prompt, you can use:
- [special characters](https://tiswww.case.edu/php/chet/bash/bashref.html#Controlling-the-Prompt) 
that can be used for current user name (`\u`), time (`\t`), host name (`\h`) etc.
- [colors](https://en.wikipedia.org/wiki/ANSI_escape_code#Colors) both for the 
foreground and the background
- unicode characters (letters, numbers, symbols, emoji)
- custom variables

Let's look at an example value of `PS1`: `\s-\v\$`
- `\s` the name of the shell
- `-` just a dash
- `\v` the version of the shell
- `$` a dollar sign at the end of the prompt

which in my terminal evaluates to: `-bash-3.2$`

To change the value of `PS1` for the current session, you can use 
`export PS1="<value>"` in the terminal:
- adding a lambda 
  ```
  export PS1="λ "
  ```
- changing the lambda's color to red 
  ```
  export PS1="\[\033[0;31m\]λ \[\033[0m\]"
  ```
- adding the current directory before the red lambda 
  ```
  export PS1="\w\[\033[0;31m\] λ \[\033[0m\]"
  ```

and so on.

If you want to make the `PS1` more permanent, you can add it to your `.bashrc` (or `.bash_profile`,
if [you're on a Mac](https://apple.stackexchange.com/questions/51036/what-is-the-difference-between-bash-profile-and-bashrc)),
reload your session with 
```
source .bashrc
``` 
et voilà!

### Getting information from git
When I work on code, I usually want to know a few things:
- am I using git? (if not, I might want to `git init`)
- what branch I am currently on?
- are there any changes in the directory - untracked, unadded, added or deleted files?

All of those things I can check by simply using the `git status` command, but it gets 
tedious to do that every time I need to change something (am I on the right branch? 
did I add everything? did I switch to a feature branch?). I can also parse git commands 
output with Bash and add the parsed information to my prompt, using custom variables. 

**Branch**

In order to get the current branch, you can parse the output of
[several different git commands](https://stackoverflow.com/questions/6245570/how-to-get-the-current-branch-name-in-git). 
I don't think there is one good way to do it, as you might want to get slightly different
results depending on the context, but some good ideas are:
- `git rev-parse --abbrev-ref HEAD` - which gives you the right name of the local branch and consistent 
`HEAD` for other contexts
- `git branch | sed -n '/\* /s///p'` - which gives you the right name of the local branch and appropriate
outputs for other contexts 

Whichever method you choose, to add it to your `PS1` prompt you can:
- add a function for parsing the branch (the `2>/dev/null` stops the command from printing the output):
  ```
  git_prompt() {
    local ref=$(git branch 2>/dev/null | sed -n '/\* /s///p')
    if [ "$ref" != "" ]
    then
      echo "($ref) "
    fi
  }
  ```
- add the function output to your prompt 
  ```
  export PS1="\w\[\033[0;31m\] λ $(git_prompt)\[\033[0m\]"
  ``` 
  or, with a hint of color 
  ```
  export PS1="\w\[\033[0;31m\] λ \[\033[0;95m\]\$(git_prompt)\[\033[0m\]\[\033[0m\]"
  ```

**Number of various changes**

You can run `git status` with various options - if you use the [short format](https://git-scm.com/docs/git-status#_short_format),
you will get the statuses of your paths that are a bit easier to understand for parsing purposes, notably:
- `?` for untracked paths
- `M` for modified paths
- `A` for added paths
- `D` for deleted paths

To parse untracked paths you can:
- grep `git status --porcelain` for `?` and count the number of lines in the output (as each untracked path
is on a new line) 
  ```
  untracked=`expr $(git status --porcelain 2>/dev/null | grep "?" | wc -l)`
  ```
- wrap it in a function that returns the number of untracked paths with a symbol of your choice when it's not 0
  ```
  function parse_untracked {
    local untracked=`expr $(git status --porcelain 2>/dev/null | grep "?" | wc -l)`
    if [ "$untracked" != "0" ]
    then
      echo " ?$untracked"
    fi
  }
  ```
- add it to your `PS1` prompt
  ```
  git_prompt() {
    local ref=$(git branch 2>/dev/null | sed -n '/\* /s///p')
    if [ "$ref" != "" ]
    then
      echo "($ref)$(parse_untracked) "
    fi
  }
  ```

You can add a separate function for each status and add them to your prompt by simply grepping for the appropriate
symbol and wrapping it in a function. For reference, my full git parsing code is 
[in this gist](https://gist.github.com/trueskawka/aab826ea75d7219790df92fba8bb8e47).

Have fun adjusting your prompt and hopefully never getting lost among your git branches!