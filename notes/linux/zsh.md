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
