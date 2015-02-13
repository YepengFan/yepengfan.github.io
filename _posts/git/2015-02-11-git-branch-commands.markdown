---
layout: post
title: "Git Branch Commands"
modified:
categories: git
description:
tags: [git]
image:
  feature: git banner white.png
  credit:
  creditlink:
comments:
share:
date: 2015-02-11T19:39:25+11:00
---
Here are some common use **git branch** commands

Browse local braches

git branch <br />
　×　dev <br />
　×　master <br />
{: .notice}
×　means the current branch

list remote branches

git branch --remote <br />
　origin/dev <br />
　origin/master <br />
{: .notice}

list all the branches including local and remote, use '-a' argument

git branch -a <br />
　×　master <br />
　 　remotes/origin/HEAD -> origin/master <br />
　 　remotes/origin/develop <br />
　 　remotes/origin/issue_193 <br />
{: .notice}

create a branch

git checkout -b new_branch
{: .notice}

This new branch forks from current branch.

checkout another branch

git checkout another_branch
{: .notice}

push local branch to remote repository

git push origin branch_name
{: .notice}

If the remote repo does not have this branch, it will create a branch automatically.

pull remote branch to local repo

git pull origin branch_name
{: .notice}

delete local branch

git checkout another_branch
git branch -d branch_to_be_deleted
{: .notice}

delete remote branch

git push origin --delete branch_name
{: .notice}

merge local branch

git merge branch_to_be_merged_in
{: .notice}

merge remote branch

git merge origin/branch_to_be_merged_in
{: .notice}
