using tmux: a screencast
------------------------
Notes from the video:

These are the vim plugins I used:

- [benmills/vimux](https://github.com/benmills/vimux)
- [edkolev/tmuxline.vim](https://github.com/edkolev/tmuxline.vim)
- [christoomey/vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator)
- [tpope/vim-dispatch](https://github.com/tpope/vim-dispatch)

```viml
" vimrc

Plugin 'benmills/vimux'
Plugin 'edkolev/tmuxline.vim'
Plugin 'christoomey/vim-tmux-navigator'
Plugin 'tpope/vim-dispatch'
```

Bash aliases I used (not talked about during screencast):

```bash
alias clr="clear && tmux clear-history"
alias tl="tmux ls"
alias ta="tmux attach-session -t"
alias t="tmux new-session -s"
```

These are the important parts of my tmux config. (Full version
[here][tmux.conf]).

```
# tmux.conf

# Smart pane switching with awareness of vim splits
# Source: https://github.com/christoomey/vim-tmux-navigator
bind -n C-h run "(tmux display-message -p '#{pane_current_command}' | grep -iqE '(^|\/)vim(diff)?$' && tmux send-keys C-h) || tmux select-pane -L"
bind -n C-j run "(tmux display-message -p '#{pane_current_command}' | grep -iqE '(^|\/)vim(diff)?$' && tmux send-keys C-j) || tmux select-pane -D"
bind -n C-k run "(tmux display-message -p '#{pane_current_command}' | grep -iqE '(^|\/)vim(diff)?$' && tmux send-keys C-k) || tmux select-pane -U"
bind -n C-l run "(tmux display-message -p '#{pane_current_command}' | grep -iqE '(^|\/)vim(diff)?$' && tmux send-keys C-l) || tmux select-pane -R"
# bind -n C-\ run "(tmux display-message -p '#{pane_current_command}' | grep -iqE '(^|\/)vim(diff)?$' && tmux send-keys 'C-\\') || tmux select-pane -l"

# clear scrollback
bind k send-keys -R \; clear-history \; send-keys Enter

# make colors work
set-option -g default-terminal screen-256color

# titles for stuff I guess
set-option -g set-titles on
set-option -g set-titles-string '[#S:#I #H] #W'

# start at 1 because 0 is on the right and that's dumb
set -g base-index 1

# vim mode keys
setw -g mode-keys vi

if-shell 'test "$(uname)" = "Darwin"' 'source ~/.tmux-osx.conf'
```

```
# tmux-osx.conf
set-option -g default-command "reattach-to-user-namespace -l bash"
```

[tmux.conf]: https://github.com/ciarand/phoenix/tree/master/dotfiles/tmux.conf