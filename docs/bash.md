# Bash

## Globbing primitives
Some globbing primitives in bash are:
- `*`  matches everything
- `?`  matches any single character
- `[abd]`  matches any character from a, b or d
- `[a-d]`  matches any character from a, b, c or d

i.e.:
- `ls *1` will list all the files that end in `1`
- `ls file[0-9]` will list all the files that start with file and end with a character from 0 to 9

## Variable interpolation
The bash shell interpolates the variable if it's in double quotes, but not if it's in single quotes.
```sh
MYSTRING="a String"
MYSENTENCE="A sentence with $MYSTRING in it"

echo $ MYSENTENCE # A sentence with a String in it

MYSENTENCE='A sentence with $MYSTRING in it'
echo $ MYSENTENCE # A sentence with $MYSTRING in it
```

## Make variables readonly
To make a variable read only put `readonly` in front of it like this:
```sh
readonly MYVAR=astring
```

## Create several files in one go
It is possible to create several files with just one command by putting them between curly braces
`touch dir/{file1,file2,file3}` will create three files under dir
