Learned from reading documentation from `vimtutor` command on wsl:ubuntu linux.
## Cursor location and file status
`CTRL-G` displays your location in the file and the file status.
`G` moves to the end of the file.
`<line-number> G` moves to that line number.
`gg` moves to the first line.
## Search command
`/` followed by a phrase searched FORWARD for the phrase.
`?` followed by a phrase searches BACKWARD for the phrase.
`n` to find the next occurrence in the same direction,
`N` to search in the opposite direction.
`CTRL-O` takes you back to older positions,
`CTRL-I` to newer positions.

`f` followed by a search character searches FORWARD for the character within a line.
`F` followed by a search character searches BACKWARD for the character within a line.
`t` followed by a search character searches FORWARD for the character within a line and positions the cursor before the character you searched for.
`T` followed by a search character searches BACKWARD for the character within a line and positions the cursor before the character you searched for.
To repeat the search function or motion across the line use `;` to repeat FORWARD and `,` to repeat BACKWARD.

`incsearch` incremental search
`hlsearch` highlight search
`:set hlsearch?` to check `hlsearch` setting.
`:set hls` and the output should be `nohlsearch` to indicate it’s off or `hlsearch` to indicate it’s on.

