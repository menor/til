## VIM

### Replacing surrounding character using vim-surround
If you have the [vim-surround](https://github.com/tpope/vim-surround) plugin installed:
- To replace surrounding symbols `cs"'` (will replace surrounding double quotes by single)
- To delete surrounding delimiters `ds(` (Will remove surrounding parentheses)

### Brackets
- `b` is a text object for a bracket so you can do things like `dab` to delete around parrenthesis instead of `da(`, `dib` to delete inside, `cib` to change inside and so on ...

### Navigation
- `{count} +` moves count lines down to the beggining of the line
- `{count} -` moves count lines up to the beggining of the line
- `gd` go to local declaration
- `*` search forward for word under the cursor
- `#` search backward for word under the cursor

### Marks
- `m{a-zA-Z}` sets a custom mark whose exact location can be accessed using `{mark}
- Certains marks are special `. jumps to the last change

### Tips
- `:windo difft` will show the diff between two windows
- `:e!` discards edits since last save and reloads the file
- `]m` moves to the next curly brace
