---
layout: post
title: tmux配置
subtitle:
tags: [环境配置,tmux]
---

```tmux
.tmux.conf

#================================================================
# General
#================================================================
bind-key c new-window -c "#{pane_current_path}"
bind-key % split-window -h -c "#{pane_current_path}"
bind-key '"' split-window -c "#{pane_current_path}"

set -g history-limit 5000 # boast history

# 修改.tumx.conf后使其生效的两种方法：
#	1，重启tmux
#	2，在tmux窗口下，先按下C-b + :，进入到命令模式后输入source-file ~/.tmux.conf，回车后生效。
# 绑定修改.tmux.conf后使其生效的快捷键为r.
bind r source-file ~/.tmux.conf \; display-message "Config reloaded.."

# 开启鼠标支持
set-option -g mouse on

set -g default-terminal "tmux-256color"

# 关闭默认的rename机制
setw -g automatic-rename off
setw -g allow-rename off 

set -g set-titles on
set -g monitor-activity on
set -g visual-activity on

#================================================================
# HotKeys
#================================================================
# change the default prefix binding of C-b to C-a.
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# 分割面板
bind - split-window -h
bind _ split-window -v

# 绑定hjkl为面板切换的上下左右键
bind -r k select-pane -U
bind -r j select-pane -D
bind -r h select-pane -L
bind -r l select-pane -R

# 绑定Ctrl+hjkl为面板上下左右调整边缘的快捷键
bind -r ^k resizep -U 5 # 绑定Ctrl+K为往上调整面板边缘5个单元格
bind -r ^j resizep -D 5 # 绑定Ctrl+J为往下调整面板边缘5个单元格
bind -r ^h resizep -L 5 # 绑定Ctrl+H为往左调整面板边缘5个单元格
bind -r ^l resizep -R 5 # 绑定Ctrl+L为往右调整面板边缘5个单元格

#================================================================
# Appearance
#================================================================
set -g base-index 1 # 设置窗口的起始下标为1
set -g pane-base-index 1 # 设置面板的起始下标为1

# 设置状态栏的颜色
set -g status-fg white
set -g status-bg black
# 设置状态栏左侧的内容和颜色
set -g status-left-length 40
set -g status-left "#[fg=green]Session: #S #[fg=yellow]#I #[fg=cyan]#P"
# 设置状态栏右侧的内容和颜色
# 15% | 28 Nov 18:15
set -g status-right "#(~/battery Discharging) | #[fg=cyan]%d %b %R"
# 每 60 秒更新一次状态栏
set -g status-interval 60

# 设置窗口列表居中显示
set -g status-justify centre

```

