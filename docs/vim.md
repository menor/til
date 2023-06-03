## VIM

### Replacing surrounding character using vim-surround
If you have the [vim-surround](https://github.com/tpope/vim-surround) plugin installed:
- To replace surrounding symbols `cs"'` (will replace surrounding double quotes by single)
- To delete surrounding delimiters `ds(` (Will remove surrounding parentheses)

### Brackets
- `b` is a text object for a bracket so you can do things like `dab` to delete around parenthesis instead of `da(`, `dib` to delete inside, `cib` to change inside and so on ...

### Insert Mode
- `I` moves to the start of the line and enters insert mode
- `A` moves to the end of the line and enters insert mode
- `x` deletes a single character
- `s` deletes a single character and goe into insert mode
- `c` is like `d` but enters insert mode after it deletes the selection

### Navigation
- `{count} +` moves count lines down to the beggining of the line
- `{count} -` moves count lines up to the beggining of the line
- `gd` go to local declaration
- `*` search forward for word under the cursor
- `#` search backward for word under the cursor
- `f` searchs forward in a line
- `F` searchs backwards in a line
- `;` move forwards to next search result in the line
- `,` move backwards to next search result in the line
- `:<line number>` moves you to a specific line number (same as `<line number>G`)
- `}` moves to the next blank line
- `{` moves to the previous blank line
- `Ctrl + u` moves half screen up
- `Ctrl + d` moves half screen down
- `%` goes to matching bracket or parenthesis

- To move through the Jumplist (each position to which your cursor jumped):
  - `ctrl-o` to move backwards 
  - `ctrl-i` to move forwards

- To move through Changelist (the position for every change you can undo):
  - `g;` to move backwards 
  - `g,` to move forwards

### Marks
- `m{a-zA-Z}` sets a custom mark whose exact location can be accessed using `{mark}
- Certains marks are special `. jumps to the last change

### Tips
- `:windo difft` will show the diff between two windows
- `:e!` discards edits since last save and reloads the file
- `]m` moves to the next curly brace

### Splits
To move between splits:
- `ctrl-w j` moves to the top split
- `ctrl-w h` moves to the left split
- `ctrl-w k` moves to the bottom split
- `ctrl-w l` moves to the right split

Once you have split your screen, how can you rearrange your splits? It’s easy with ctrl-w:

- `ctrl-w J` moves the active to the top
- `ctrl-w H` moves the active split to the left
- `ctrl-w K` moves the active split to the bottom
- `ctrl-w L` moves the active split to right
- `ctrl-w r` rotates splits to the right/down
- `ctrl-w R` rotates splits to the left/up

## Plugins

### Comment.nvim
- `gcc` comments line using line comment syntax
- `gbc` comments line using block comment syntax
- `gc[count]{motion}` line comment the region contained in {motion}
- `gb[count]{motion}` block comment the region contained in {motion}

- `gco` puts you in insert mode in a comment line below
- `gcO` puts you in insert mode in a comment line above
- `gcA` puts you in insert mode at the end of the line

