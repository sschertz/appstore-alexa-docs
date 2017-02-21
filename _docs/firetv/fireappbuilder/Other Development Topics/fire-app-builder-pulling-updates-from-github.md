---
title: Pull Updates from Github
permalink: fire-app-builder-pulling-updates-from-github.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

You can stay updated about the latest commits to Fire App Builder by watching and starring the project in Github. You can also periodically check the [Release Notes][fire-app-builder-release-notes] page in the documentation.

{% include note.html content="If this is the first time you're cloning and building the project, see [Download Fire App Builder and Build an App][fire-app-builder-download-and-build] instead of this topic. (This topic addresses how to apply updates to an existing project.)" %}

* TOC
{:toc}

## Getting Updates

When new versions of Fire App Builder are released and pushed out to Github, you can get the new version's updates and integrate the code into your project using the common `git pull` technique used with Github workflows. 

First change to your fire-app-builder directory (or whatever you've named it). Then pull the latest changes into your repository:

```
cd fire-app-builder
git pull
```

When you run a [`git pull`](https://git-scm.com/docs/git-pull) command, git downloads the latest updates from the original repository and attempts to merge the updates into your code. 

## Resolving Merge Conflicts

If changes in the original repository's files conflict with changes you've made to your local copy, git will not automatically overwrite your local copies with the updates. Instead, git will show you merge conflicts for the affected files and remove the affected files from its tracking. You will then need to resolve the merge conflicts, add the files back into git tracking, and commit your update.

After you run `git pull`, if you see merge conflicts, pen the files with merge conflicts. Carrots (>>>>>>) and (<<<<<<) indicate areas where conflicts occur. Manually edit the files to remove the carrots. Then add the file back into git tracking with `git add <filename>`, where <filename> is the name of the file you're adding back in. 

See [Resolving a merge conflict from the command line](https://help.github.com/articles/resolving-a-merge-conflict-from-the-command-line/) for details on how to resolve merge conflicts.

## Git Resources

Here are some good resources for learning git:

* [git's documentation](https://git-scm.com/doc)
* [git - the simple guide](http://rogerdudler.github.io/git-guide/)
* [git Tutorials from Atlassian](https://www.atlassian.com/git/tutorials/)

{% include links.html %}