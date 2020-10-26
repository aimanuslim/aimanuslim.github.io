---
title: Git Rebase vs Git Merge
date: 2020-10-26T05:46:30.732Z
description: Explanation found on reddit about git rebase vs git merge
---
Comment taken from: <https://www.reddit.com/r/learnprogramming/comments/4ykgu4/eli5_what_is_git_rebase_and_common_use_cases_for/d6oe0hx?utm_source=share&utm_medium=web2x&context=3>

Internally, git is structured based on a kind of data structure known as a [graph](https://www.topcoder.com/community/data-science/data-science-tutorials/introduction-to-graphs-and-their-data-structures-section-1/) -- more precisely, a specific kind of graph known as a [Directed Acyclic Graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph), or DAG for short.

In essence, a "graph" is a kind of structure where you have a bunch of arbitrary nodes with arbitrary connections between each other. These connections may or may not be bidirectional, depending on whatever you think is more useful for whatever you're trying to do. A DAG is like a graph, except you're not connect nodes in such a way that you can loop back to a previously visited node -- there are no cycles.

In the case of Git, each commit represents a "diff" -- a record of whatever changes you need to make to transform the codebase from what it looks like before the commit to after. The commits are like nodes, and are connected to each other in the order they need to be applied.

If you only ever commit to your master branch, your DAG will look like this:

```
A ---> B ---> C ---> D [master]
```

...where A, B, C, and D are commits, and commit D is labeled as the "master" branch.

Git supports something called "branching", where you can do something like this:

```
A ---> B ---> C ---> D [master]
       \
        +----> X ---> Y ---> Z [experiment-1] 
```

Here, what's happened is that after commit B, we made a *branch* called "experiment-1". In this branch, we added commits X, Y, and Z which are completely independent from the commits that were added to the master branch.

(Perhaps we wanted to play around with an experimental and potentially risky feature without breaking any working code in the master branch? So that we, if experiment-1 is broken, we don't piss off our coworkers who would prefer to work with non-broken code).

What rebasing lets us do is take the graph that looks like the above, and make it look something like this:

```
A ---> B ---> C ---> D [master]
                      \
                       +----> X ---> Y ---> Z [experiment-1] 
```

Basically say something like "you know how in order to get to experiment 1, I need to apply the diffs inside commits A, B, X, Y, Z in that order? Yeah, I changed my mind -- I want to split off of a different commit so that now in order to get to experiment 1, you need to apply diffs A, B, C, D, X, Y, and Z, in that exact order".

There are many potential applications to this (since git is a data structure, you can do arbitrarily complex/creative/nasty things to the underlying DAG), but the two usual use cases are when...

1. Somebody commits something really nice to the master branch (like a bug fix!) and you'd really like to have that in your work-in-progress.
2. You're happy with the results of your experiment, and want to add it to master, but don't want to do a merge, since that could look messy/you like having master be a straight line without splits.

More specifically, if you start from here:

```
A ---> B ---> C ---> D [master]
        \
         +----> X ---> Y ---> Z [experiment-1] 
```

...and `merge` experiment-1 into master and update the master branch, you'll end up with your DAG looking like this:

```
A ---> B ---> C ---> D -------> M [master]
        \                      /
         +----> X ---> Y ---> Z [experiment-1] 
```

...where M is a new commit representing what happens when you combine diffs X, Y, and Z.

In contrast, if you `rebase` experiment-1 into master and update the master branch, you'll end up with your DAG looking like this:

```
A ---> B ---> C ---> D
                      \
                       +----> X ---> Y ---> Z [experiment-1, master]
```

...which is exactly identical to this:

```
A ---> B ---> C ---> D ---> X ---> Y ---> Z [experiment-1, master]
```

So, if you'd prefer keeping an exact historical record, you might go with merges, but if you prefer keeping an idealized and clean historical record, you might go with rebasing.

To learn more, this is a good tutorial: <http://learngitbranching.js.org/>