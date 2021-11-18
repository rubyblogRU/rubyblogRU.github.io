---
layout: post
title:  "Алиасы в .bashrc для работы с git в *NIX терминале"
date:   2019-10-06 09:00:00 +0300
categories: others
---

```text
alias gl='git log --pretty=format:"%an | %ad | %h | %s" --date=short'
alias gs='git status'
alias ga='git add -A'
alias gc='git commit -m'
alias gb='git branch -v'
alias gh='git hist --all'
alias gt='git tag'

alias gm='git merge'
alias gmgtl='git mergetool'
alias gmm='git merge master'

alias gbd='git branch -d'
alias gbt='git branch --track'
alias gchb='git checkout -b'
alias gchf='git checkout -f'
alias gdump='git cat-file -p'
alias gitssh='git remote set-url origin'
alias gitsshtest='git remote -v'
alias go='git checkout'
alias gob='git checkout -b'
alias god='git checkout -b develop'
alias gor='git checkout -b release'
alias gp='git push -u orign master'
alias gr='git reset HEAD'
alias gre='git rebase'
alias grh='git reset --hard'
alias grm='git rm --cached'
alias groh='git revert HEAD --no-edit'
alias gtype='git cat-file -t'
alias cola='git-cola'
```