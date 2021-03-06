---
layout: post
title: Github Pages and Yard
---

A quick post to document how I set up my yard documentation to use with Github pages.  For an open source project I've been working on called [switch_gear](https://github.com/allcentury/switch_gear) I wanted to host my code documentation on Github pages.

Github has a few options for this.  If you go to the repo settings, you'll see the area in question:

![github](https://s30.postimg.org/fk8fyl6k1/Screen_Shot_2017-05-25_at_3.08.22_PM.png)

Here I've set this project to render a page from the `/docs` folder on the master branch.  Normally however when you execute:

```shell
$ yardoc
```

The output is actually put in the directory `doc`.  In order to change that you need to run:

```shell
$ yardoc -o docs
```

Now that we've got the right folder we can push up to Github (either on a feature branch or directly on master).  It can take a few minutes but you should see your yard documentation served.

In order to automate this, I created a git hook that lives in your local project under `.git/hooks/pre-push`. Make sure you give executable permissions to that new file:

```sh
$ chmod u+x .git/hooks/pre-push
```

Then in that file paste:

```sh
#! /bin/bash

echo "Running documentation"

files=$(git diff --name-only)
yardfiles=$(sed 's/ /,/g' <<< $files)
yard doc -o docs $yardfiles

echo "Including doc changes in a separate commit"

git add docs/
git commit -m "Updated documentation"
```

Now with every push you'll ensure your documentation is re-rendered for files changed.  There is a downside to this though, something I haven't been able to figure out yet.  When yard generates docs it puts a timestamp in the HTML that git picks up as new changes.  Ideally we could suppress those file changes if that's all that changed.  Looking at Yard's source, it appears you'd have to write your own templates to make that work.  I'll update this post if I go down that route but for now this is good enough for me.

Thanks for reading.
