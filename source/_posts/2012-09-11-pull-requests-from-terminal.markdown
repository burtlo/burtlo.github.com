---
layout: post
title: "Pull Requests from Terminal"
date: 2012-09-11 09:30
comments: true
categories: 
---

Several companies are using Github as a source repository solution. Many teams have adopted a great workflow that involves opening a pull request when a feature is complete. Using Github to code review a new feature or fix is awesome . Anything that promotes more communication and review of ones code is awesome.

The process goes something like this:

* Start a new feature branch
* Implement feature within the branch
* Push the branch to Github
* View completed branch within Github
* Open pull request for new branch

Reviewing the steps I noted the primary tools used to accomplish them:

* (terminal,git)    - Start a new feature branch
* (terminal,editor) - Implement feature within the branch
* (terminal,git)    - Push the branch to Github
* (browser)         - View completed branch within Github
* (browser)         - Open pull request for new branch

What stood out to me was how the final stages of this process stood out. It was then that I sought to remedy that.

With the help of the [hub](http://defunkt.io/hub/) and a little bash I created a single command to allow me to perform a pull request from the command-line.

```bash
function pull-request {
  hub pull-request -h $(__github_remote_origin):$(__github_current_branch)
}

function __github_current_branch {
  echo "$1`git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/'`"
}

function __github_remote_origin {
  echo "$1`git remote -v | grep "(push)" | sed "s#origin.*:\([^/]*\).*push.*#\1#"`"
}
```

Hub's `pull-request` command by itself was great when I was working within my own projects but failed to generate the correct pull request targets when I was working on a project that belonged to an organization.

This small change allows me to combine the last two steps of the workflow into a single step as well as bring it more in-line with the other tools in the entire process.


### Installation

Install [hub](http://defunkt.io/hub/) from brew.

```
brew install hub
```

Open your `.bash_profile` or `.bashrc` and add the following lines:

```
function pull-request {
  hub pull-request -h $(__github_remote_origin):$(__github_current_branch)
}

function __github_current_branch {
  echo "$1`git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/'`"
}

function __github_remote_origin {
  echo "$1`git remote -v | grep "(push)" | sed "s#origin.*:\([^/]*\).*push.*#\1#"`"
}
```

Save and exit. Run the source command to use your updated bash commands.

```
source
```

Running the pull-request command should launch your GIT_EDITOR or EDITOR to compose the pull request message.