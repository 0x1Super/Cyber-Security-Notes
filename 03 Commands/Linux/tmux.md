# create or add this to ur ~/.tmux.conf 

- `Configuration`

```
# set prefix
set -g prefix C-s
bind C-a send-prefix
unbind C-b

set -g history-limit 100000
set -g allow-rename off

bind-key j command-prompt -p "Join pan from:" "join-pane -s :'%%'"
bind-key s command-prompt -p "Send pane to:" "join-pane -t :'%%'"

set-window-option -g mode-keys vi

run-shell /opt/tmux-logging/logging.tmux

# Set new panes to open in current directory

bind c new-window -c "#{pane_current_path}"
bind '"' split-window -c "#{pane_current_path}"
bind % split-window -h -c "#{pane_current_path}"
```
-------------------------------------------------------
- `usage`
ctrl + b to type commands

tmux new -s name
% to open a vertical pane
"  to open horizontal pane 
arrow keys to move between panels 
c new window 
numbers to  change windows 
CTRL + [ copy mode]
? find up
/ find down
space copy mode and select enter 
CTRL ] past 

CTRL + ALT +SHIFT + P to log my session
CTRL + hold CTRL and arrows to resize 
CTRL space change places 
Alt . last word typed 

CTRL + V visual block mode
G copy whole colum 
v delete 
