---
layout: post
title: "nb - a zsh function to change or create new git feature branches (with completion)"
tags:
 - git
 - zsh
---

`nb` (<strong>n</strong>ew <strong>b</strong>ranch) is my very simplified method of creating new feature og fix branches in git.

When I'm working on fixes or features in an existing project, I check out a new branch. This also makes it way easier to create easily understood [pull-requests][] when you need to get it reviewed.

````sh
# nb.zsh
nb () {
  if [[ $# > 0 ]]
  then
    if [[ $(git branch | tr -d '* ' | grep "$1") != "" ]]
    then
      git checkout $1
    else
      git checkout master && git checkout -b $1
    fi
  else
    git branch | tr -d '* '
  fi
}
# completion
_nb() { reply=($(git branch | tr -d '* ' | xargs echo)) }
compctl -K _nb nb
````

Without any arguments it just lists the current, existing, local branches. These are the ones it will tab-complete. If the argument is an existing branch then check it out, if it's not then create it based on master.

I'm using zsh and I'm not sure what it would take to make it work in bash. Probably not too much.

[pull-requests]: /2013/10/14/one-shot-command-to-create-a-github-pull-request-and-open-it-in-your-browser.html
