---
title: tmux + Tmuxinator  打造你的炫酷终端分屏环境
date: 2016-12-20 12:58:36
type: "categories"
categories: "vim"
tags:
---

## tmux

tmux 是一个终端分屏软件，你是否还在感叹终端窗口不够用? 有了它，你就可以不用操心了。并且可以运行在服务端，就算ssh链接断开了也没事，下次登录，在链接上会话就可以继续前面的进度工作了。
对于拥有大屏的你简直爽到爆啊!

![Alt Text](https://camo.githubusercontent.com/9b914113e5e2377e9d7fa93112c0639f398bb8ba/68747470733a2f2f662e636c6f75642e6769746875622e636f6d2f6173736574732f3134313231332f3931363038342f30363566656637632d666538322d313165322d396332332d6139363232633764383363332e706e67 ) 

> 配置文件

```conf
tmux source-file ~/.tmux.conf
```

> 基本快捷键

* tmux new -s work   新建会话
* tmux ls 显示当前已经创建的会话
* tmux a -t name 链接会话
* tmux kill-session -t work 关闭指定 session
* tmux kill-server 关闭所有 session

* c+b l 最近使用的window
* c+b ; 最近使用的panel
* c+b " 横向分割窗口，创建 pane
* c+b 方向键  切换pane
* c+b , 重命名window
* c+b c 新建window
* c+b ? 查看按键绑定
* c+b Q 关闭window
* c+b z toggle最大化pane
* c+b d 退出并保存会话
* c+b s 切换会话

> 复制到系统剪切板

如果你用item2，需要开启mouse report

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//16-12-21/35255665-file_1482308304418_115b9.png ) 


> <=2.0 配置示例

```conf
setw -g mode-mouse on
set -g mouse-select-pane on

 #setw -g mode-keys vi

 #关闭自动命名
 set-option -g allow-rename off

 #分屏操作
 bind | split-window -h
 bind - split-window -v

 #设置终端颜色
 set -g default-terminal "screen-256color"
 set -g default-terminal "screen-256color-bce"
 #设置esc延迟
 set -sg escape-time 10

 #旋转pane
 bind r rotate-window
 bind q break-pane
 bind Q kill-window
```

> 2.0 及其以上配置示例

```conf
set -g mouse on
#setw -g mode-keys vi  

#关闭自动命名
set-option -g allow-rename off

#分屏操作
bind | split-window -h
bind - split-window -v

#设置终端颜色
set -g default-terminal "screen-256color"
set -g default-terminal "screen-256color-bce"
#设置esc延迟
set -sg escape-time 10

#旋转pane
bind r rotate-window
bind q break-pane
bind Q kill-window


#光标shape
set -g -a terminal-overrides ',*:Ss=\E[%p1%d q:Se=\E[2 q'

#鼠标滚屏
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'select-pane -t=; copy-mode -e; send-keys -M'"  
bind -n WheelDownPane select-pane -t= \; send-keys -M 

```

> 命令行模式

类似 vim，输入:后，进入命令模式，可以完成大部分操作 如`source-file ~/.tmux.conf`

## tmuxinator

可以快速创建 tmux 的窗口分布, 管理 tmux 的会话

> 安装

```shell
gem install tmuxinator

# 写到 bashrc 或者 zshrc 里面，设置默认编辑器
export EDITOR='vim'
```

> 基本快捷键

其实这些操作都是编辑的.tmuxinator/ 下的配置文件，可以直接修改

* tmuxinator n work    创建工程配置
* tmuxinator o work    打开指定配置文件

> 基本配置语法

```conf
# o 命令打开时的工程名称
name: sample
# 所有命令的根目录
root: ~/
# 窗口设置
windows:
  - editor:
      layout: main-vertical
      panes:
        - vim
        - guard
  - server: bundle exec rails s
  - logs: tail -f log/development.log
```

> 自定义窗口布局

默认有几种 layout, 开启tmux后，可以使用快捷键prefix space切换layout，建议开启4个Pane进行测试。

* even-horizontal
* even-vertical
* main-horizontal
* main-vertical
* tiled

自定义 layout 步骤:

*  在 tmux 中调整好窗口的位置信息，执行`tmux list-windows`，会出现位置信息参数。
*  将其拷贝到配置文件的 layout 参数，`注意去掉前面那节`

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//16-12-21/32534178-file_1482309129558_2186.png ) 

>  一个 pane 执行多个命令

可以用&& 符号链接命令,或者如下

![Alt Text](http://o99eh3ii0.bkt.clouddn.com//16-12-21/23997236-file_1482309214236_c2e6.png ) 


