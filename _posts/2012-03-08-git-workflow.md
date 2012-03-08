---
layout: post
title: "Git Workflow"
category: developer
tags: [git, developer]
---
{% include JB/setup %}

The new workflow model for Git commits follows the model presented with
[git-flow](https://github.com/nvie/gitflow). As mentioned on the mailling
list, there are two primary branches with the authoritive repository on
[github](https://github.com/openxpki/openxpki):

master
: The stable, production branch containing officially-released versions

develop
: The unstable, development branch that may or may not work

These branches are managed by the OpenXPKI team members that have the role
"integrator". More on this in a moment.

In addition to the above branches, the developers should use their own
branches for active development and push the commits to their personal
repositories on github. They then send a "pull request" to the project and
an integrator will pull the commits and merge them into the "develop" and "master"
branches, as appropriate.

To simplify communications among team members, we suggest the following naming
conventions (these happen to also be the defaults in git-flow):

feature/*name*
: Development commits regarding a specific commit -- branch gets merged to develop

release/*version*
: Last-minute fixes for a pending official release -- branch gets merged to master

hotfix/*name*
: Bug fix needed outside of regular development workflow -- merged to master

The idea here is that when the developer is working on a new feature, the
first step is to branch from "develop" to a new "feature/*name*" branch. When
complete, the developer pushes the results to github and sends a pull request
to the integrators.

The integrator has the task of pulling the feature commits and merging them to
the develop branch. Then, when it is time for an official release, the integrator
branches from "master" to a new "release/*version*" branch, merges the commits
from "develop" and, after QA testing and bugfixes, merges the results back to
"master".

The added step of the integrator may seem like overhead and it does add a step, 
but it also helps stabilize and formalize the release process.

The "hotfix/*name*" works similarly to the "feature/" and "release/" branches,
but allows us to apply bug fixes to the master branch without having to
worry about the status of the "develop" branch.


