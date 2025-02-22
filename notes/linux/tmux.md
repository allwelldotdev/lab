tmux is a terminal multiplexer. It allows you to;

- Create multiple terminal sessions within a single window.
- Split your terminal into pages.
- Create new windows within a session.
- Detach from and reattach to sessions, allowing you to keep processes running even when you close your terminal.

Some basic tmux commands are;

- Start a tmux ‘unnamed’ session: `tmux`
- Create a new window: `Ctrl-b c`
- Switch between windows: `Ctrl-b n` (next) or `Ctrl-b p` (previous)
- Split pane vertically: `Ctrl-b %`
- Split pane horizontally: `Ctrl-b "`
- Switch between panes: `Ctrl-b arrow keys`
- Detach from session: `Ctrl-b d`
- List sessions: `tmux ls`
- Reattach to a session: `tmux attach -t [session name]` or `tmux a -t [session name]`
- Resize panes `Ctrl-b Ctrl-arrow keys`
- Close current pane: `Ctrl-b x`
- Detach from session: `Ctrl-b d`
- Enter command mode: `Ctrl-b :`
- Re-run source file: `Ctrl-r`

For more on tmux, check out my [Claude chat](https://claude.ai/chat/09865328-1438-4739-805a-b9c99729393d).

Some useful commands you can use in command mode:

- `new-window`: Creates a new window
- `rename-window name`: Renames the current window
- `kill-window`: Closes the current window
- `split-window`: Splits the current pane into two
- `resize-pane -D 10`: Resizes the current pane down by 10 cells
- `set mouse on`: Enables mouse support
- `source-file ~/.tmux.conf`: Reloads your tmux configuration file