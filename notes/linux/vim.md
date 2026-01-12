Learned from:
1. reading documentation from `vimtutor` command on wsl:ubuntu linux,
2. watching Jason Cannon's course, titled 'Vim Masterclass', on [Udemy](https://www.udemy.com/course/vim-commands-cheat-sheet/).
## Cursor location and file status

- `CTRL-G` displays your location in the file and the file status.
- `G CTRL-G` add word and byte count to the display of `CTRL-G`.
- `G` moves to the end of the file.
- `<line-number> G` moves to that line number.
- `<line-number> gg` moves to that line number.
- `<line-percentage> %` moves to the line percentage.
- `gg` moves to the first line.

## Navigation commands
- `w` move forward one *word* at a time (including punctuation marks).
- `W` move forward one *word* at a time (excluding punctuation marks).
- `b` move backward one *word* at a time (including punctuation marks).
- `B` move backward one *word* at a time (excluding punctuation marks).

- `CTRL-F` move forward one page.
- `CTRL-B` move backward one page.
- `Z ENTER` move position of cursor on line to the top.
- `ZZ` move position of cursor on line to the center.
## Search command

- `/` followed by a phrase searched FORWARD for the phrase.
- `?` followed by a phrase searches BACKWARD for the phrase.
- `n` to find the next occurrence in the same direction,
- `N` to search in the opposite direction.
- `CTRL-O` takes you back to older positions,
- `CTRL-I` to newer positions.
---
- `*` cycles (i.e. searches) forward through instances of a word the cursor is under within a file.
	- `n` and `N` then move the cursor to the next search.
- `#` cycles (i.e. searches) backward through instances of a word the cursor is under within a file.
	- `n` and `N` then move the cursor to the next search.
---
- `f` followed by a search character searches FORWARD for the character within a line.
- `F` followed by a search character searches BACKWARD for the character within a line.
- `t` followed by a search character searches FORWARD for the character within a line and positions the cursor before the character you searched for.
- `T` followed by a search character searches BACKWARD for the character within a line and positions the cursor before the character you searched for.
- To repeat the search function or motion across the line use `;` to repeat FORWARD and `,` to repeat BACKWARD.

- You can also use `[counts]` together with these too like `2f Space` to find the next `Space` after `2` places. These are motions and can also be used with *as motions* with operators. Like below:
- `dtw` deletes text from the cursor to the beginning of `w` in the line.
- `d2tw` adds `[count]` by deleting text 2 counts from the cursor to the beginning of `w` in the line.
---
- `incsearch` incremental search.
- `hlsearch` or `hls` highlight search.
- `:set hlsearch?` to check `hlsearch` setting. If output is `nohlsearch` then `hlsearch` is off.
- `:set hls` or `:hls` or `:hlsearch` set `hlsearch` to on.
---
- `d/<search-term>` deletes text/lines from the cursor position to the end of the search term `<search-term>` whether on the same line or on a different line.
- `"a y /<search-term>` yanks all text from current cursor position up to the search term. Typing `:reg a` will should the text yanked into the `"a` register.

>A neat trick for search, replace, and repeat incrementally is `/search-term` to search a word or character, `cw` to change the word, `n` to move to the next similar search-term, and `.` to automatically repeat the `cw` action.
## Matching parenthesis search

`%` while the cursor is on `( )`, `[ ]`, or `{ }` goes to its match.
## Substitute command

>This command does the same thing 'find and replace' does. It searches for a string or character and replaces it with another.

- `:s/old/new <ENTER>` only substitutes the first occurrence of “thee” in the line.
- `:s/old/new/g <ENTER>` substitutes globally in the line.
- `:#,#s/old/new/g` where #,# are the line numbers of the range of lines where the substitution is to be done.
	- You could also use special symbols that represent the line number in the file, like:
		- `.` represents current line
		- `$` represents end of file
			- `.,$s/old/new/g` `<ENTER>` substitutes from current line to end of file.
		- `%` represents all lines or whole file.
			- `%` could also be `1,$` as in from line `1` to end of file `$`.
		- `/<search-term-1>/,/<search-term-2>/s/old/new/g` substitutes from lines that fall between `search-term-1` and `search-term-2`.
			- This method can be combined with other symbols too like:
			  `/<search-term>/,$s/old/new/g` substitutes from lines that fall between `search-term` and end of file `$`.

> Want to substitute **`/var/spool/mail`** to **`/usr/local/mail`**?
> 
> You could use a backslash (`\`) to escape the forward slashes (`/`) - this is the hard way. Or you could use a different method which constitutes as the easier way. Below's an example:
> 
> - **the hard way**
> 	- `:s/\/var\/spool/\/usr\/local/`
> - **the easy way**
> 	- `:s#/var/spool#/usr/local#`

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

## Deleting text

- `x` delete text (each char).
	- `x` in visual mode deletes the selection (same as `d`).
- `dd` delete one line of text.
- `2dd` delete two lines of text.
## Retrieving and merging files

> You can also read the output of an external command. For example, `:r !ls` reads the output of the `ls` command and puts it below the cursor.

- `:r <FILENAME>` retrieves disk file FILENAME and puts it below the cursor position.
- `:r !ls` reads the output of the `ls` command and puts it below the cursor position.
## Open command

- `o` to open a line BELOW the cursor and start Insert mode.
- `O` to open a line ABOVE the cursor.

## Insert mode
- `i` lets you insert text before the cursor.
- `Shift-i` or `I` lets you insert text at the beginning of the line.

> A neat trick with `i`/`INSERT` mode is you can combine count with it. See this example: If you want to create a text of multiple asterisks `*`, say 5, use the command `5i`, during `INSERT` mode type `*` then `Esc`. You'll see 5 asterisks.

## The append command

> `a`, `i` and `A` all go to the same Insert mode, the only difference is where the characters are inserted.

- `a` to insert text AFTER the cursor.
- `A` to insert text after the end of the line.
- `e` command moves (or motions) the cursor to the end of a word.
## Replace mode

> Replace mode is like Insert mode, but every typed character deletes an existing character.

- `r` lets you replace a single text.
- `R` lets you enter replace mode until `ESC` is pressed.

## UPPERCASE & lowercase
- `~` toggles the case of a letter.
- `g~w` toggles the letter case of words starting from the position of the cursor.
- `g~~` toggles the letter case of words in a line.

- `gUw` toggles the letter case of words to *UPPERCASE*, with the `w` word motion, starting from the position of the cursor.
- `gUU` toggles the letter case of words in a line to *UPPERCASE*.
- `guw` toggles the letter case of words to *lowercase*, with the `w` word motion, starting from the position of the cursor.
- `guu` toggles the letter case of words in a line to *lowercase*.

## Joining
- `J` joins two lines together. It's smart about this procedure. First, it appends a space to the end of the line before appending the new line. Second, if the presiding line has a period `.` it appends two spaces.
- `gJ` joins two lines without appending any space.

## Copy and paste text

- `y` operator yanks (copies) text
- `p` puts (pastes) text.

> You can also use `y` as an operator: `yw` yanks one word, `yy` yanks the whole line, then `p` puts that line.

## Registers
Registers in Vim are similar to clipboards.
### Register types
- Unnamed
	- `""`
	- `""` holds text from `d`, `c`, `s`, `x`, and `y` operations.
	- `""` contains last operated on text.
- Numbered
	- `"0 "1 ... "9`
	- `"0` holds last text yanked (`y`).
	- `"1` holds last text deleted (`d`) or changed (`C`).
	- They shift with each `d` or `c`.
- Named
	- `"[custom]` this is when you custom name the register when using `y`, or `p`
	- e.g. `"a yy`: `"a` is a custom register.
	- `"a` ...`"z` an example of named registers.

### Register commands
- In `NORMAL` mode
	- `"2 yy` yanks line into `"2` register
	- `"1 p` puts (pastes) text from `"1` register
- In `INSERT` mode
	- `CTRL-R "2 p` puts (pastes) text from `"2` register

- `:reg` list all registers.
- `:reg [register(s)]` list one or more registers.
	- `:reg z` list everything in `z` registers.
	- `:reg 1az` list everything in `1`, `a`, and `z` registers.

### Repeating with Registers
- `[count][register]operator` OR
- `[register][count]operator`

> A cool example with registers and other commands like `cw` is we can change a word and use the changed word elsewhere by appending a named (custom) register to the `cw` command. Like so: `"a cw` this changes a word and puts the changed word in the `"a` register for later use.

> You can append to named registers by starting the command with the capital letter of the register. For instance; `"Fyy` appends the newly yanked text to the text in `"f` register.

## Set option

`:set xxx` sets the option `xxx`.

Some options are:
- '`ic`' '`ignorecase`' ignore upper/lower case when searching
- '`is`' '`incsearch`' show partial matches for a search phrase
- '`hls`' '`hlsearch`' highlight all matching phrases.

Prepend "`no`" to switch an option off: `:set noic`

> If you want to ignore case for just one search command, use `\\c` in the phrase: `/ignore\\c <ENTER>`

> You can toggle an option using `!` at the end of the option like `:set nu!` to toggle on page numbers if they are initially off and `:set nu!` to toggle off page numbers if they are initially on.
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
- `daw` = delete *around* word
- `ciw` = change *in* word.
- `viw` = visually highlight *in* word

for sentences…
- `das` = delete *around* sentence
- `cis` = change *in* sentence
- `vis` = visually highlight *in* sentence

for paragraphs…
- `dap` = delete *around* paragraph
- `cip` = change *in* paragraph
- `vip` = visually highlight *in* paragraph

for functions...
- `dif`  = delete *in* function
- `cif`  = change *in* function
- `vif` = visually highlight *in* function.

for grouped items
- `di[` = delete *in* brackets `[...]`
- `ci(` = change *in* parenthesis `(...)`
- `vi{` = visually highlight in braces `{...}`

for HTML tags
- `dat` = delete *around* tag
- `cit` = change *in* tag
- `vit` = visually highlight *in* tag

> Did you know, text objects also work with yanking and registers? Yes, they do. Here's an example:
> `"l yit` yanks text *in* tag into the `l` register. 

## Macros
Macros in Vim allow you to record keystrokes (which are saved in registers) and play them back again thereby making repeated changes across your files.
The only limitation is that you can't make an existing macro record a new macro.

- `q` followed by a register `a` (or `b` or any alphabet) starts recording a macro into the register. `q` stops the recording. Now the recorded keystrokes can be found at `"a` register (or the register it was recorded into).
- `@a` replays the recorded keystrokes in register `"a`.
- `@@` replays the most recently executed macro.

### Macro Best Practices
- Normalise the cursor position.
	- `0`. 
	- Always start your macros off at the right cursor position.
- Perform edits and operations.
- Position your macros to enable easy replays.
	- `j`.
	- Always end on a new line.

> Recorded registers can be appended to using `q` and the capital letter of the register. For example, if the register was `a`, it'll be `A`, if `b` it'll be `B` and vice versa.
> The command is `qC` to append to keystrokes in the `"c` register.

Remember, macros just replay what's in the register. Meaning, you can edit the content in the register by;
- pasting it `"a p`,
- editing it,
- and yanking it `"a yy` or cutting it `"a dd`
back into the same named register.
Only make sure you understand the representation of the symbols so you'll be editing it correctly without errors when the macro is replayed.

Macros are quite powerful when used correctly across texts with the same pattern.

### Saving Macros
- You can save your macros for future use in the `~/.viminfo` file.
	- `_viminfo` for Windows.
- It stores history and non-empty registers.
- Read when vim starts.
- Can easily overwrite registers.
- A more reliable place to store your macros for future use is the `~/.vimrc` file (which is also used for Vim configurations).
	- `_vimrc` for Windows.

You can save macros into the `~/.vimrc` file like so:
```bash
# Everything in '...' is the macro.
let @d = ':s/"/'/g^M:s/(//^M:s/)//^M:s/ =>/,/^M$vi'gUj'
```
To type `Escape` in `INSERT` mode in Vim, use `Ctrl-v`, then key press the region on the keyboard and it'll fill it up in text like `^[` for `Escape`.

> In case you're wondering there's no specified way to "delete" named registers from the `:registers` list, however you can clear or empty the register using:
>
> - `qaq`
> - `:let @a = ''`
> - `:call setreg('a', '')`

Remember,
- macros are a recorded series of keystrokes,
- macros use registers,
- repeating a macro: `[count]@[REGISTER]`
- saving macros in `~/.vimrc` file: `let @[REGISTER] = 'keystrokes'`
---
This is a cool macro `0f@lv/<^Mhy0Pldatf<,i^M^[` that allows you turn this blob of text:

```
<a href="#">@armyspy.com</a><a href="#">@cuvox.de</a><a href="#">@dayrep.com    </a><a href="#">@einrot.com</a><a href="#">@fleckens.hu</a><a href="#">@gust    r.com</a><a href="#">@jourrapide.com</a><a href="#">@rhyta.com</a><a href="#    ">@superrito.com</a><a href="#">@teleworm.us</a>
```

into this: (*To get desired result, type this to replay macro:* `10@<macro-register>` *or* `10@@`)

```
armyspy.com
cuvox.de
dayrep.com
einrot.com
fleckens.hu
gustr.com
jourrapide.com
rhyta.com
superrito.com
teleworm.us
```

A much quicker way to achieve the same result as above is this macro: `df@f<cf>^M^[`

The power of macros!

## Visual mode
Visual mode in Vim allows you to highlight series of text just like you would with a mouse. There are 3 types of visual mode:
1. Character-wise visual mode; started with `v`
2. Line-wise visual mode; started with `V`
3. Block-wise visual mode; started with `Ctrl-v`

You can use motions to expand the visual area.
You can also use text objects to expand the visual area.

Some objects you can use in visual mode include:
- `~` - Switch case
- `c` - Change
- `d` - Delete
- `y` - Yank
- `r` - Replace
- `x` - Delete
- `I` - Insert
- `A` - Append
- `J` - Join
- `u` - Make lowercase
- `U` - Make uppercase
- `>` - Shift right
- `<` - Shift left.

> To improve your understanding of visual mode check out this video lecture again: https://www.udemy.com/course/vim-commands-cheat-sheet/learn/lecture/6593354#overview

> When you perform a shiftwidth operation using either `>` or `<` it shifts text by the shiftwidth setting, which you can find using `:set shiftwidth?`. The setting defaults to `8`, which means each time you shift text it's shifted by 8 characters, on most Vim instances.
> 
> Also by default, the `tabstop` setting is set to `8` as well. The `tabstop` setting is the number of spaces a tab character in a file counts for (said another way, `tabstop` is the width of a tab).
> 
> Another setting that involves tabs is the `expandtab` setting. The `expandtab` setting by default is also set to `noexpandtab` meaning it's disabled by default. When enabled, it inserts the appropriate number of spaces instead of an actual tab character.
> 
> To make tabs easy to spot turn on `list` mode using `:set list <ENTER>`. This setting shows the tab and space characters.

You can also use visual mode with command mode `:` by highlighting the text you want to apply your command to and pressing `:`. This outputs `:'<,'>` in the command section of vim. Now any setting or command you input here would affect the highlighted text. Some command examples you can append to `:'<,'>` are:
- `center` or `ce` or `center <width>` or `ce<width>`
- `left` or `le` or `left <width>` or `le<width>`
- `right` or `ri` or `right <width>` or `ri<width>`
- `normal <command>` to run normal-mode commands on highlighted text. Commands include:
	- `@<register>` to apply a macro on the highlighted text

## Vim settings, preferences and customizations
- `~/.vimrc` Vim's user-wide config file
	- rc = run commands
	- System-wide vimrc and personal vimrc
	- Unix/Linux/Mac file location: `~/.vimrc`
	- Windows file location: `$HOME/_vimrc`
	- Each line is executed as a command
		- `set ruler` = `:set ruler`
	- To see options that have been set to a value other than their default value, type `:set <ENTER>`.
	- You can also map keys and keystrokes using the `map` command, like so: `map <KEY> <KEYSTROKES>`. Here's some keystrokes you can include in your map command.
		- ![map command in Vim](../assets/Pasted%20image%2020251121162620.png)
		- Check out this video for details on mapping keys in Vim: https://www.udemy.com/course/vim-commands-cheat-sheet/learn/lecture/6598102#overview
	- To prevent custom mapped keys from clashing with Vim system keys, you can use the default `<leader>` key provided by Vim as a prepend to your custom keys.
		- The default leader key provided by Vim is `\`.
		- Setting `map <leader>w :w!<CR>` which maps the "write to file" `:w! <ENTER>` to `\w`
		- If you don't want to use the default leader key provided by Vim, you can always remap to another key, like so: `let mapleader=","` to remap to `,` as the new leader key.
		- To learn more on the topic: `:h mapping`
		- Watch this to learn some advanced Vim settings like `mkvimrc`, `vmap`, `nmap`, `vnoremap`, and `inoremap`: https://www.udemy.com/course/vim-commands-cheat-sheet/learn/lecture/6598102?start=541#notes

## Buffers
When you open a file Vim reads the content of the file you see into a *buffer*. This is a temporary location. 
- `:buffers`, `:files`, `:ls` displays a list of files in the buffer
- `:bnext` or `:bn` moves to the next file in the buffer
- `:bprevious` or `:bp` moves to the previous file in the buffer
- `:blast` or `:bl` moves to the last file in the buffer
- `:bfirst` or `:bf` moves to the first file in the buffer
- `:b2` moves to the 2nd buffer in the buffer list
- `Ctrl-Shift-6` switches between the previous and current buffers in the buffer list

- When you display a buffer with `:ls`, `%a` shows the active buffer, `#` shows the alternate buffer (the buffer you'd switched from).
	- You can take advantage of this and use `:b#` to explicitly move to the alternate buffer.

- A buffer can be in one of three states:
	- active `%a`
	- hidden `#h`
	- inactive (i.e. not loaded into memory)

- The `|` pipe command in Vim is the command separator.
- `E` or `Explore` lets you open the explore tab where you can view files.

## Windows
In Vim, a window is a view of a buffer.
- `:split` or `:sp` or `Ctrl-w s` Splits the window in two horizontally.
	- `:sp <filename>` splits windows horizontally with the new window opening the file indicated by `filename`.
- `:vsplit` or `:vs` or `Ctrl-w v` Splits the window in two vertically.
	- `:vs <filename>` splits windows vertically with the new window opening the file indicated by `filename`.
- `:q` or `Ctrl-w q` while focusing on a window, closes the window.
- `Ctrl-w Ctrl-w` switches between windows.
- `Ctrl-w h/j/k/l` switches between windows in direction of `h`, `j`, `k`, or `l`.
- `:on` or `:only` or `Ctrl-w o` closes every other open window except the current window (i.e. the window in focus).
---
Resizing the shape of windows:
- `Ctrl-w +` increases the height of a window
- `Ctrl-w -` decreases the width of a window
- `Ctrl-w <` increases the width of a window to the left
- `Ctrl-w >` increases the width of a window to the right
- `Ctrl-w |` maximizes the height of a window
- `Ctrl-w _` maximizes the width of a window
- `Ctrl-w =` equalizes the size of all windows.
---
Rearranging windows:
- `Ctrl-w r` rotates the windows in a left to right direction
- `Ctrl-w R` rotates the windows in the opposite direction (from right to left)
- `Ctrl-w H/J/K/L` moves the focused window to the side of `H`, `J`, `K`, or `L`
---
Buffer and Windows commands:
- `:ba` or `:ball` opens all the buffers into their own windows. If there's not enough space for windows, Vim will open as many as possible and tell you there isn't enough on your screen for the others.
- `:bufdo` lets you execute commands on all of your buffers.
	- `:windo` does something similar but with windows.
---
Finding help on `Ctrl-w` commands:
- `:h ^ww <ENTER>` takes you to help manual.

## Explore mode

---
On Zed:
From what I found on Zed, the options of `:Explore` in Vim are not fully *explored* in Zed and perhaps for good reason (as fully exploring them may hamper the use of Zed's own settings).

Commands that work on Explore mode in Vim and also on Zed:
- `d` create a new directory
- `%` create a new file
- `-` move cursor to parent directory
- `D` delete a file (not sure of deleting a directory - will have to confirm)
- 

## Neovim
These settings also work on **Zed**, the programming IDE, as well.

---
- `:set wrap` and `:set nowrap` to enable and disable text wrapping.
---
Horizontal scrolling commands:

Once `nowrap` is set, you can use the following commands to scroll horizontally:
- `zh`: Scrolls the view one character to the left, effectively moving the text one character to the right.
- `zl`: Scrolls the view one character to the right, effectively moving the text one character to the left.
- `zH`: Scrolls the view half a screen width to the left.
- `zL`: Scrolls the view half a screen width to the right.
- `zs`: Scrolls the view so the cursor is at the start (left edge) of the window.
- `ze`: Scrolls the view so the cursor is at the end (right edge) of the window.
---
- `:cpplink` lets you copy the line of the code on GitHub for sharing (if the project has been pushed to GitHub).
- `gA` shows all references of a particular symbol
- `gd` go to definition of a particular symbol.
- `*` or `Shift-8` select/highlight the symbol the cursor on top of, then travel/move your cursor across representations of the same symbol. This opens a *multi-buffer* in Zed.
	- `Shift-8` or `*` or `f3` circles through the selections.
	- `Shift-n` or `Shift-f3` circles backwards through the selections.
	- `zM` folds the *multi-buffer*.
	- `zR` unfolds the *multi-buffer*.
- `g/` global symbol search across project/directory
	- You can do some things in global search using regex. Learn more here: https://youtu.be/ltE30UmsBlY?t=478
- `[[` or `]]` to navigate through functions or classes.
- `[m` or `]m` to navigate to the previous and next method definition.
---
How Vim keys work with Zed and Git:
- `dO` stages a change. Pressing it again unstages the change.
- `do` shows diff of staged change.
- `[c` or `]c` to navigate through chunks of git changes.
	- You can even use the `.` command to "repeat" a Git staging action.
- `:G` or `Ctrl-Shift-g` opens the git window.
	- `Alt-y` stage highlighted file
	- `Shift-Alt-y` unstage highlighted file
	- `Ctrl-space` stage all
	- `Shift-Ctrl-space` unstage all
	- `Ctrl-w l` or `Ctrl-w h` moves sideways between Git window and Git file.
	- `Ctrl-enter` to bring up or enlarge the edit box.
		- Same command to commit after inserting commit message.
		- `Shift-Ctrl-enter` to amend previous commit with new commit.
	- `Alt-l` to autocomplete with AI

> Not a bad Git workflow with Zed.

- `mb` marks a spot in the code file which you can return to again with `back-tick b`. Like as below:
  ```
  `b
  ```
- Important to note that `:vsplit` and `:split` work also in Zed. To open a file in the split, type absolute file path relative to project root. For instance:
  ```
  :vsplit backend/src/utils/aiUtils.ts <ENTER>
  ```
- `[x` or `]x` increments or decrements the selection.
---
Working with the AI panel in Zed:
- `:AI` toggles focus on the AI panel.
	- `Ctrl-Alt-b` or `Ctrl-Shift-/` does the same thing with slight differences.
		- `Ctrl-Alt-b` toggles focus between the AI panel and editor window, closing the AI panel as it leaves focus.
		- `Ctrl-Shift-/` toggles focus between the AI panel and editor window, NOT closing the AI panel as it leaves focus (instead leaving the window open). Interestingly, this is what `:AI` does too.
- Bear in mind, AI mode has Vim mode as well.
- `@` gives options of numerous inserts to be used with AI mode.
- `Ctrl-Alt-/` brings up different AI models.
- `Ctrl-Shift-.` or `Ctrl->` after highlighting some lines of code forwards those lines to AI panel for inference.
- `Ctrl-x Ctrl-a`, while in Vim `INSERT` mode in the editor, opens the generate inline editor. 
---
To learn about the use of debuggers in Zed, check out: https://youtu.be/ltE30UmsBlY?t=1574
 
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
