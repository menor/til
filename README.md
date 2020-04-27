# til
📘 Today I Learned


## VIM

## Bash

### Globbing primitives
Some globbing primitives in bash are:
- `*`  matches everything
- `?`  matches any single character
- `[abd]`  matches any character from a, b or d
- `[a-d]`  matches any character from a, b, c or d

i.e.:
- `ls *1` will list all the files that end in `1`
- `ls file[0-9]` will list all the files that start with file and end with a character from 0 to 9

### Create several files in one go
It is possible to create several files with just one command by putting them between curly braces
`touch dir/{file1,file2,file3}` will create three files under dir

## Firefox

You can create different profiles to store separate history, bookmarks and add-ons, this is useful for keeping a separate work and life profile, so you don't think about live while working (or about work while living), also separate add-ons might come handy in case you want to test in a vanilla browser. To set this up in FIrefox got to `about:profiles`

## Homebrew
You can see which packages are outdated by running `brew outdated` and then upgrade them all using `brew upgrade`, you can also do this with casks by doing `brew cask outdated` and `brew cask upgrade`

