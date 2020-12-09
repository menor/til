# GIT

## Git Config
Since git 2.13, it is possible to use different configs depending on the directory using conditional includes.

An example:

Global config `~/.gitconfig`

```
[user]
    name = John Nada
    email = john@nada.tld

[includeIf "gitdir:~/work/"]
    path = ~/work/.gitconfig
```

Work specific config `~/work/.gitconfig`

```
[user]
    email = john.nada@acme.ltd
```

## Magic rebase
- Seen [here](https://stackoverflow.com/a/54824509)

This works like a charm for when you have a lot of commits to rebase and some of them are giving you conflicts, it is a pain cause you need to fix the same conflict over and over. Here is a solution using `git commit-tree`

- Make a new branch and do a standard merge
`git checkout -b temp`
`git merge origin/master`

- You will have to resolve conflicts, but only once and only real ones.
- Then stage all files and finish merge.
`git commit -m "Merge branch 'origin/master' into 'temp'"`

- Then return to your branch (let it be alpha) and start rebase, but with automatical resolving any conflicts.
`git checkout alpha`
`git rebase origin/master -X theirs`

- Branch has been rebased, but project is probably in invalid state. That's OK, we have one final step. We just need to restore project state, so it will be exact as on branch 'temp'. Technically we just need to copy its tree (folder state) via low-level command git commit-tree. Plus merging into current branch just created commit.

`git merge --ff $(git commit-tree temp\^{tree} -m "Fix after rebase" -p HEAD)`

- And delete temporary branch
`git branch -D temp`

That's all. We did a rebase via hidden merge.

## Git checkout
- To go to the previous branch you were before you can do `git checkout -`
