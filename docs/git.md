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
    email = john.nada@acme.tld
```


