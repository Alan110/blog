---
title: 美化你的vim
date: 2016-09-11 01:03:12
type: "categories"
categories: "vim"
tags:
---


## 光标

将vim的光标改成插入模式下是竖线

> vim + tumx + item2 + macOS

```
# .vimrc
if exists('$ITERM_PROFILE')
  if exists('$TMUX') 
    let &t_SI = "\<Esc>Ptmux;\<Esc>\<Esc>]50;CursorShape=1\x7\<Esc>\\"
    let &t_SR = "\<Esc>Ptmux;\<Esc>\<Esc>]50;CursorShape=2\x7\<Esc>\\"
    let &t_EI = "\<Esc>Ptmux;\<Esc>\<Esc>]50;CursorShape=0\x7\<Esc>\\"
  else
    let &t_SI = "\<Esc>]50;CursorShape=1\x7"
    let &t_SR = "\<Esc>]50;CursorShape=2\x7"
    let &t_EI = "\<Esc>]50;CursorShape=0\x7"
  endif
end
```

> neovim + tmux 

```
# init.vim
let $NVIM_TUI_ENABLE_CURSOR_SHAPE=1

# tmux.conf
set -g -a terminal-overrides ',*:Ss=\E[%p1%d q:Se=\E[2 q'

```

> [其他环境下配置](http://vim.wikia.com/wiki/Change_cursor_shape_in_different_modes)

## 状态栏

> [airline](https://github.com/vim-airline/vim-airline)

![pic alt](http://o99eh3ii0.bkt.clouddn.com//16-9-11/88702776.jpg)

airline是一个状态栏美化插件。使用vundle安装

```
Plugin 'vim-airline/vim-airline'
Plugin 'vim-airline/vim-airline-themes'
```

> [安装powerline](https://powerline.readthedocs.io/en/latest/installation.html#pip-installation) 使用符号字体

![](http://o99eh3ii0.bkt.clouddn.com//16-9-11/63571441.jpg)

安装步骤:

```
1. pip3 --user install powerline-status   // 安装powerline

2. cd ./fonts && ./install.sh             // 下载字体，安装字体

3. let g:airline_powerline_fonts = 1      // 配置vimrc

4. 设置终端字体为powerline字体
```

[powerline-fonts下载](https://github.com/powerline/fonts)

![pic alt](http://o99eh3ii0.bkt.clouddn.com//16-9-11/37487394.jpg)

注意点:

* pip安装必须要用 —user属性，不然会很卡, 这里貌似是有bug

* 设置终端字体，此处2个字体都要修改

> 切换airline主题

一般主题也会自带airline主题，所以切换一下就行了。

```
:AirlineTheme {theme-name}
```


## 主题

> [gruvbox](https://github.com/morhetz/gruvbox)

![pic alt](http://o99eh3ii0.bkt.clouddn.com//16-9-11/49414541.jpg)

安装

```vimL
Plug 'morhetz/gruvbox' "run :PlugInstall
colorscheme gruvbox

"切换model
set background=dark    " Setting dark mode
set background=light   " Setting light mode
```

配置:
* [usage](https://github.com/morhetz/gruvbox/wiki/Usage)
* [configration](https://github.com/morhetz/gruvbox/wiki/Usage)


> [jellybeans](https://github.com/nanotech/jellybeans.vim)

![pic alt](http://o99eh3ii0.bkt.clouddn.com//16-9-11/40179806.jpg)

