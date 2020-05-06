## Figuring where you are

* [`git log`](https://git-scm.com/docs/git-log) - show all commits
* [`git branch`](https://git-scm.com/docs/git-branch) - shows current branch
* [`git status`](https://git-scm.com/docs/git-status) - show files with changes
  - `git st` is a common alias

## Looking at changes

* [`git diff`](https://git-scm.com/docs/git-diff) - the diff since last commit
* `git diff BRANCH-NAME` - the diff from another branch
* [`git show`](https://git-scm.com/docs/git-show) - show the diff of last commit
* `git show BRANCH-NAME` - show the diff of `BRANCH-NAME`

## Staging changes

Use [`git add`](https://git-scm.com/docs/git-add)

* `git add -A`/`git add .`/`git add -a`
  - Don't use these. (except maybe your first commit)
* `git add FILENAME_OR_DIR`, this is better
* `git add -p`
  - stages in _hunks_
  - allows you to see each line of diff while deciding what to include
* [`git reset`](https://git-scm.com/docs/git-reset) to un-stage your changes

## Committing changes

If you have staged your changes properly all you need is
* [`git commit`](https://git-scm.com/docs/git-commit)
  - ex `git commit -m 'add person.js'`
  - use **present tense** for your messages
* `git commit --amend` to add to previous commit
  - Use this if you forgot to add something to your last commit
  - If you have pushed before doing this, you will need force-push

## Pulling

* [`git fetch`](https://git-scm.com/docs/git-fetch) - don't use this
  - gets the changes of a branch/branches
* [`git pull`](https://git-scm.com/docs/git-pull) - use this
  - gets the changes _and_ updates your branches accordingly

## Pushing

Use [`git push`](https://git-scm.com/docs/git-push)

* `git push origin BRANCH-NAME`
* `git pushf origin BRANCH-NAME`
  - add the alias `pushf = push --force-with-lease` to `.gitconfig` if you don't have it
  - Never `push` if you don't need to
  - Always make sure you know what you are overriding! `git diff origin/BRANCH-NAME`


## Debugging history

One of the many great things about `git` is it makes diagnosing issues easy.

Check these out !!

* [`git blame`](https://git-scm.com/docs/git-blame)
  - `git blame FILENAME` - see history of file chages
  - add alias `praise = blame` for positivity!
* [`git bisect`](https://git-scm.com/docs/git-bisect)
  - This is effectively a _binary search_ to find a commit that broke something



## Checking out branches

Use [`git checkout`](https://git-scm.com/docs/git-checkout)

* `git checkout BRANCH-NAME`
* `git checkout -` go back to last branch
* `git co` is a common alias


## Adding a feature

* Do NOT commit directly to master
* Check out a branch from master `git checkout my-new-feature`
  - where `my-new-feature` is the branch name
* Add your code and commit `git commit -m 'add my new feature'`
* Push your branch to GitHub `git push origin my-new-feature`
* Make a PR on GitHub
* Review the changes
* Make requested updates, commit again
* Merge PR into master from github



## Things to remember

To make sure you don't lose your work always keep these things in mind

* Commit often - keep your commits small
* Push often - once every few commits
* Look at the diff from origin before force-pushing
* Do not push to master. Make a PR
* **Look at [the docs](https://git-scm.com/docs/)**.  Do not just copy-paste from Stack Overflow without knowing what it does



### The Golden Rule

*__Don't change what's been shared__*

- Do not (accidentally) change committed history that has been pushed to `origin`

- If you do, make sure to either undo the change or ensure that all team members have a consistent history before proceeding

### Review

First, let's call to mind some of the `git` commands we've already seen.

What does each one do?  Are there similarities in basic functionality and code/objects handled by each command?

Each command will typically either be used to inspect the changes at a particular layer(s) or it will transition a set of changes from one layer to another.

Command | Function | Data Layer(s)
----- | ----- | -----
add | move changes | WD -> Staging
commit | move changes | Staging -> History
status | inspect changes | WD/Staging/History (via what commit is the last one)
show | inspect changes | WD/History (if `SHA1` is given)
branch | inspect | WD/History (via what branch is current)
diff | inspect changes | WD<-->History (by default other possibilities exist but this is the main one)
log | inspect | History
push | move changes/sync | Local History -> Remote History
pull | move changes/sync | Remote History -> Local History/WD/Current Branch
fetch | move changes/sync | Local History -> Local History(background)
checkout | move index | WD (moves WD reference to a different HEAD of History)
merge | move changes | applies new changes from one branch to the HEAD of the current branch
rebase | move changes | applies changes from a different branch's history underneath the current branch's history
reset | un-set changes | History -> Staging/WD/Null depending on what flag is set

__NOTE:__ `git log --oneline --decorate --graph --all` is your friend



Let's look at an example and outline the steps up to committing and pushing to origin:

#### Feature Branching + Merging

#### Feature Branch

As a project grows, it can help substantially to break out sets of changes into their own branches which are subsequently merged back in to the `master` or `dev` branch.  As you know, these branches can also be pushed to github.


#### Conflict Resolution

Feature branches are great but can lead to difficulties when overlapping or incompatible sets of changes are merged back in to a common branch, e.g., `master`.  `Git` is pretty good about safely handling multiple streams of changes, but sometimes you have to manually pitch in to get the job done.

When trying to `git merge` that produces a conflict the output will look something like this:

```bash
Auto-merging convo.js
CONFLICT (content): Merge conflict in convo.js
Resolved 'convo.js' using previous resolution.
Automatic merge failed; fix conflicts and then commit the result.
```

> To see the beginning of the merge conflict in your file, search the file for the conflict marker <<<<<<<. When you open the file in your text editor, you'll see the changes from the HEAD or base branch after the line <<<<<<< HEAD. Next, you'll see =======, which divides your changes from the changes in the other branch, followed by >>>>>>> BRANCH-NAME.
[source](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/)


- Resolve conflict by either removing the utility lines (a rarely available solution but it works in this case), or remove one set of changes.  Either way, whatever the state of the file when we save+quite, that will be what ends up being committed so make sure it's valid!


## Merging and rebasing branches

### merging

[`git merge`](https://git-scm.com/docs/git-merge)

Typically branches, should be _merged_ into master.  **This should be done from Github**.  Merging takes every commit and along with a merge-commit from the branch and adds that to the target branch (usually `master`)

**Ex**: From `master`: `git merge BRANCH-NAME` takes all the commits from `BRANCH-NAME` and a merge-commit and adds those to master.


### rebasing

[`git rebase`](https://git-scm.com/docs/git-rebase)

Rebasing rewrites history.  This adds the commits from another branch and puts your commits on top of your branch.  (Actually it puts _new copies_ of your commits on top). Typically, we rebase `master` from another branch.  This does not add an extra merge-commit.

**Ex**: From some branch: `git rebase master` will take anything that was added to master since branched off (or last rebased) and put those commits _before yours_.  Your commits are then added on top of your branch.

Technically, `git pull` is a shorthand for `git fetch origin HEAD` together with `git merge origin/HEAD`.  In other words, `pull` is an alias for fetching the changes from origin and merging them into the local copy of the branch.  adding the `--rebase` flag to `pull` will rebase rather than merge, thereby not adding a merge commit to your history but carrying with it additional pain when conflicts emerge.



## Resets (Resources)
`git reset` can be used to undo committed history and place the changes back either into the staging area `--soft` or in the working directory `--mixed` or discard them entirely `--hard`.  Be very careful with `git reset` especially with the `--hard` option since this is potentially destructive.

If you undo public history you will have to `git push --force` after making the changes.

[How to Reset (almost) anything](https://github.com/blog/2019-how-to-undo-almost-anything-with-git)

[Reset, Checkout, Revert](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting)



## Extra Resources


- [Creating a PR from a branch](https://help.github.com/articles/creating-a-pull-request/)

- [Appriving/Merging a PR](https://help.github.com/articles/merging-a-pull-request/)

- [An Incredible Git Tutorial](http://gitimmersion.com/) probably the second most helpful git thing I've ever come across . . .by our friend `Jim Weirich`

- [a nice set of cheat sheets](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet) from Atlassian

- [A more in depth and practical look at git rebase](https://dev.to/maxwell_dev/the-git-rebase-introduction-i-wish-id-had) Helpful to strengthen your rebase sorcery

- [Linus Torvalds nerding out about git](https://www.youtube.com/watch?v=4XpnKHJAok8) Buckle up

- [Obligatory Junio Hamano interview](https://www.youtube.com/watch?v=qs_xS1Y6nGc)
