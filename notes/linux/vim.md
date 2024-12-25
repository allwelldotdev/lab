Learned from:
1. reading documentation from `vimtutor` command on wsl:ubuntu linux,
2. watching Jason Cannon's course, titled 'Vim Masterclass', on [Udemy](https://www.udemy.com/course/vim-commands-cheat-sheet/).
## Cursor location and file status

- `CTRL-G` displays your location in the file and the file status.
- `G` moves to the end of the file.
- `<line-number> G` moves to that line number.
- `gg` moves to the first line.
## Search command

- `/` followed by a phrase searched FORWARD for the phrase.
- `?` followed by a phrase searches BACKWARD for the phrase.
- `n` to find the next occurrence in the same direction,
- `N` to search in the opposite direction.
- `CTRL-O` takes you back to older positions,
- `CTRL-I` to newer positions.
---
- `f` followed by a search character searches FORWARD for the character within a line.
- `F` followed by a search character searches BACKWARD for the character within a line.
- `t` followed by a search character searches FORWARD for the character within a line and positions the cursor before the character you searched for.
- `T` followed by a search character searches BACKWARD for the character within a line and positions the cursor before the character you searched for.
- To repeat the search function or motion across the line use `;` to repeat FORWARD and `,` to repeat BACKWARD.
---
- `incsearch` incremental search.
- `hlsearch` or `hls` highlight search.
- `:set hlsearch?` to check `hlsearch` setting. If output is `nohlsearch` then `hlsearch` is off.
- `:set hls` or `:hls` or `:hlsearch` set `hlsearch` to on.

>A neat trick for search, replace, and repeat incrementally is `/search-term` to search a word or character, `cw` to change the word, `n` to move to the next similar search-term, and `.` to automatically repeat the `cw` action.
## Matching parenthesis search

`%` while the cursor is on `( )`, `[ ]`, or `{ }` goes to its match.
## Substitute command

>This command does the same thing 'find and replace' does. It searches for a string or character and replaces it with another.

- `:s/thee/the <ENTER>` only substitutes the first occurrence of “thee” in the line.
- `:s/thee/the/g <ENTER>` substitutes globally in the line.
- `:#,#s/old/new/g` where #,# are the line numbers of the range of lines where the substitution is to be done.
- `:%s/old/new/g` to change every occurrence in the whole file.
- `:%s/old/new/gc` to find every occurrence in the whole file, with a prompt whether to substitute or not.
## Executing external commands

>You can execute any external command this way, also with arguments.
>All `:` commands must be finished by hitting `ENTER`

`:! <command>` executes an external command.

```
  Some useful examples are:
     (Windows)        (Unix)
      :!dir            :!ls            -  shows a directory listing.
      :!del FILENAME   :!rm FILENAME   -  removes file FILENAME.
```

## Writing files

`:w <FILENAME>` writes the current vim file to disk with name 'FILENAME'.
### Selecting text to write to file

> Pressing `v` starts Visual selection. You can move the cursor around to make the selection bigger or smaller. Then you can use an operator to do something with the text. For example, `d` deletes the text.

`v` motion `:w <FILENAME>` saves the visually selected lines in file 'FILENAME'.
## Retrieving and merging files

> You can also read the output of an external command. For example, `:r !ls` reads the output of the `ls` command and puts it below the cursor.

- `:r <FILENAME>` retrieves disk file FILENAME and puts it below the cursor position.
- `:r !ls` reads the output of the `ls` command and puts it below the cursor position.
## Open command

- `o` to open a line BELOW the cursor and start Insert mode.
- `O` to open a line ABOVE the cursor.
## The append command

> `a`, `i` and `A` all go to the same Insert mode, the only difference is where the characters are inserted.

- `a` to insert text AFTER the cursor.
- `A` to insert text after the end of the line.
- `e` command moves (or motions) the cursor to the end of a word.
## Replace mode

> Replace mode is like Insert mode, but every typed character deletes an existing character.

`R` enters replace mode until `ESC` is pressed.
## Copy and paste text

- `y` operator yanks (copies) text
- `p` puts (pastes) text.

> You can also use `y` as an operator: `yw` yanks one word, `yy` yanks the whole line, then `p` puts that line.
## Set option

`:set xxx` sets the option `xxx`.

Some options are:
- '`ic`' '`ignorecase`' ignore upper/lower case when searching
- '`is`' '`incsearch`' show partial matches for a search phrase
- '`hls`' '`hlsearch`' highlight all matching phrases.

Prepend "`no`" to switch an option off: `:set noic`

> If you want to ignore case for just one search command, use `\\c` in the phrase: `/ignore\\c <ENTER>`
## Get help

Vim has a comprehensive on-line help system. To get started, try one of these three:

- press the `<HELP>` key (if you have one)
- press the `<F1>` key (if you have one)
- type `:help <ENTER>`

Read the text in the help window to find out how the help works.

- `CTRL-W CTRL-W` to jump from one window to another.
- `:q <ENTER>` to close the help window.

You can find help on just about any subject, by giving an argument to the "`:help`" command. Try these (don't forget pressing `<ENTER>`):

```
    :help w
    :help c_CTRL-D
    :help insert-index
    :help user-manual
```
## Startup script

Enable Vim features.

Vim has many more features than Vi, but most of them are disabled by default. To start using more features you should create a "vimrc" file.

1. Start editing the "vimrc" file. This depends on your system:
    1. `:e ~/.vimrc` for Unix
    2. `:e ~/_vimrc` for Windows
2. Now read the example "vimrc" file contents:
    1. `:r $VIMRUNTIME/vimrc_example.vim`
3. Write the file with:
    1. `:w`

The next time you start Vim it will use syntax highlighting.

You can add all your preferred settings to this "vimrc" file.

For more information type `:help vimrc-intro`

## Command completion

Command line completion with `CTRL-D` and `<TAB>`

1. Make sure Vim is not in compatible mode: `:set nocp`
2. Look what files exist in the directory: `:!ls or :!dir`
3. Type the start of a command: `:e`
4. Press `CTRL-D` and Vim will show a list of commands that start with "`e`".
5. Type `d<TAB>` and Vim will complete the command name to "`:edit`".
6. Now add a space and the start of an existing file name: `:edit FIL`
7. Press `<TAB>`. Vim will complete the name (if it is unique).

> Completion works for many commands. Just try pressing `CTRL-D` and `<TAB>`. It is especially useful for `:help` .

>When typing a `:` command, press `CTRL-D` to see possible completions.
>Press `<TAB>` to use one completion.

## Text objects

for words…
- `daw` = delete a word
- `ciw` = change inner word.

for sentences…
- `das` = delete a sentence
- `cis` = change inner sentence.

for paragraphs…
- `dap` = delete a paragraph
- `cip` = change inner paragraph.
## Navigational key-bindings for zsh

```bash
# Navigation
bindkey '^a' beginning-of-line       # Ctrl+A: Move to beginning of line
bindkey '^e' end-of-line             # Ctrl+E: Move to end of line
bindkey '^b' backward-char           # Ctrl+B: Move backward one character
bindkey '^f' forward-char            # Ctrl+F: Move forward one character
bindkey '^[b' backward-word          # Alt+B: Move backward one word
bindkey '^[f' forward-word           # Alt+F: Move forward one word

# Editing
bindkey '^u' backward-kill-line      # Ctrl+U: Delete from cursor to start of line
bindkey '^k' kill-line               # Ctrl+K: Delete from cursor to end of line
bindkey '^[d' kill-word              # Alt+D: Delete word forward
bindkey '^[^h' backward-kill-word    # Alt+Backspace: Delete word backward
bindkey '^y' yank                    # Ctrl+Y: Paste last deleted text
bindkey '^t' transpose-chars         # Ctrl+T: Swap current character with previous

# History
bindkey '^s' history-incremental-search-forward  # Ctrl+S: Forward history search
bindkey '^[p' history-search-backward            # Alt+P: Search backward in history
bindkey '^[n' history-search-forward             # Alt+N: Search forward in history
bindkey '^[<' beginning-of-buffer-or-history     # Alt+<: Move to first line in history
bindkey '^[>' end-of-buffer-or-history           # Alt+>: Move to last line in history

# Completion
bindkey '^i' expand-or-complete      # Tab: Perform completion
bindkey '^[[Z' reverse-menu-complete # Shift+Tab: Reverse completion menu

# Misc
bindkey '^l' clear-screen            # Ctrl+L: Clear screen
bindkey '^x^e' edit-command-line     # Ctrl+X, Ctrl+E: Edit command in external editor
```
## Command line window

`q:` show command line window in vim. To quit it, type `:q`.
## Further reading

Read the user manual next: "`:help user-manual`".

For further reading and studying, this book is recommended: Vim - Vi Improved - by Steve Oualline Publisher: New Riders The first book completely dedicated to Vim. Especially useful for beginners. There are many examples and pictures. See [http://iccf-holland.org/click5.html](http://iccf-holland.org/click5.html) This book is older and more about Vi than Vim, but also recommended:

Learning the Vi Editor - by Linda Lamb Publisher: O'Reilly & Associates Inc. It is a good book to get to know almost anything you want to do with Vi. The sixth edition also includes information on Vim.
