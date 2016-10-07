---
title: 增强neovim(vim)的代码编辑体验
date: 2016-09-13 15:56:29
type: "categories"
categories: "vim"
tags:
---

原生的vim，编辑效率已经很高，但要让编程变得轻松，甚至享受，还需要一些神级插件的加持。有了这些插件，vim的编辑体验完全不输于任何IDE

## 代码完成 

YouCompleteMe + ultisnips + superTab + tern_js

这恐怕是身为一个代码编辑器最重要的能力了。vim自带有miniComplete，效果一般且只能手动触发。其他代码完成插件本人也尝试过很多这里也不提了，直接上终极插件

> [YouCompleteMe](https://github.com/Valloric/YouCompleteMe)

![pic alt](http://o99eh3ii0.bkt.clouddn.com//16-9-15/53439318.jpg)

youCompleteMe是一个非常出色的vim代码完成引擎，支持大部分语言，可以结合别的语言分析引擎，比如**tern**(js语义分析引擎),编译的时候添加对应的编译属性

* 对c，c++ c#等语言支持最好,支持c族语言的语法检查,跳转到变量声明，查找引用等
* 自动补全出现过，或者打开过的buffer里的字符串，无需按快捷键,支持模糊匹配
* 当按下.  / -> / :: 时，进行语义补全，提示各种功能函数，变量
* 支持文件路径补全
* 可以与UltiSnips集成,在提示列表中显示snips，并补全

##### 安装
使用插件管理器安装，Vundle/Pathogen/vim-plug,我用的是vim-plug
`Plug 'Valloric/YouCompleteMe'`
如果手动git下载，请确保执行`git submodule update --init --recursive`

##### 编译
`cd ~/.vim/bundle/YouCompleteMe`
`./install.py --tern-completer`
可以加上别的语言编译器属性，写js的话就加上`--tern-completer`。或者啥都要'--all'。详情请看官方文档

##### 注意
* 如果更新了插件，需要重新编译
* vim版本在7.3.598以上,支持Python 2 或者 Python 3,没有的话，重新编译一个vim

##### 配置tern
在工程目录下创建.tern-project文件，基础配置如下，详情可以看下tern的[官方文档](http://ternjs.net/doc/manual.html#configuration)
```javascript
{
  "libs": [
    "browser",
    "jquery"
  ],
  "plugins": {
        "node": {}
    }
}
```
( tern_js 和YCM是可以分开使用的，不用编译的时候带上，但要全局npm 安装。在我的测试来看，我感觉编译带上后，提示要快很多。而且tern-project的配置不要也可以完成提示，而且是自动根据语义提示是browser还是jqeury还是node。看到这里的朋友可以注意一下,不清楚是不是都是这样。 )

##### 兼容ultisnips Tab补全

默认情况下，YCM和ultisnips都是用tab补全，会有冲突。目前有2个解决方案。第一个就是给其中一个换一个trigger_key,这样2者完全不会冲突。第二个方案就通过superTab做桥梁，兼容二者。

` Plug 'ervandew/supertab' `
```vimScript
" make YCM compatible with UltiSnips (using supertab)
let g:ycm_key_list_select_completion = ['<C-n>', '<Down>']
let g:ycm_key_list_previous_completion = ['<C-p>', '<Up>']
let g:SuperTabDefaultCompletionType = '<C-n>'

" better key bindings for UltiSnipsExpandTrigger
let g:UltiSnipsExpandTrigger = "<tab>"
let g:UltiSnipsJumpForwardTrigger = "<tab>"
let g:UltiSnipsJumpBackwardTrigger = "<s-tab>"
```
____

## 代码检查

neomake + eslint

> [neomake](neomake/neomake)

neomake是一个异步执行:make的插件,使用neovim的job control功能。常见的用途就是结合代码检查插件，进行异步检查,避免阻塞vim UI。类似[Syntastic]()。vim也能使用，不过没有异步的能力。

##### 安装
`Plug 'neomake/neomake'`
`autocmd! BufWritePost * Neomake`
`let g:neomake_javascript_enabled_makers = ['eslint']`
**注意** 需要全局npm安装eslint

> eslint

eslint是一个ECMAScript/JavaScript代码检查工具，类似JSHint/JSLint,但是配置更灵活，检查更强大。

##### 安装
`npm install -g eslint`
在工程目录下执行`eslint --init`,进入交互式配置，生成.eslintrc.json配置文件
##### 配置

eslint的配置主要有3个概念 -- env , rules , globals
**env**
```javascript
{
    "env": {
        "browser": true,
        "node": true
    }
}
```
是环境配置，代表了一套检查规则,可以快速配置。eslint自带了这些环境配置
![pic alt](http://o99eh3ii0.bkt.clouddn.com//16-9-15/44680460.jpg)

**globals**
可以指定全局变量以避免eslint检查报错undefined
```javascript
{
    "globals": {
        "var1": true,
        "var2": false
    }
}
```

**rules**
详细配置每一条规则，每条规则的报错级别有3个可选项，必选其一。有些规则还有别的属性。全部规则请看[官方文档](http://eslint.org/docs/rules/)。

默认所有规则都是关闭的，可以配置`"extends": "eslint:recommended"`属性，开启常用rules。

* "off" or 0 - turn the rule off
* "warn" or 1 - turn the rule on as a warning (doesn’t affect exit code)
* "error" or 2 - turn the rule on as an error (exit code is 1 when triggered)

```javascript
{
    "rules": {
        "eqeqeq": "off",
        "curly": "error",
        "quotes": ["error", "double"]
    }
}
```

**语言版本**
`parserOptions`选项配置ecma的版本，已经其他扩展能力，如jsx

```javascript
{
    "parserOptions": {
        "ecmaVersion": 6,
        "sourceType": "module",
        "ecmaFeatures": {
            "jsx": true
        }
    },
    "rules": {
        "semi": 2
    }
}
```

## 文件/代码查找
fzf + ag
> [fzf](https://github.com/junegunn/fzf)

fzf是一个用go语言写的命令行模糊查询工具。可以与vim结合使用。它的速度很快。
fzf是一个通用筛选工具，你可以自己指定它的数据源，然后用fzf来筛选。

##### 安装
`sudo brew install fzf`
直接在命令行下执行 `fzf`就会查询当前目录下的所有文件。但是当你选择一个文件按下回车时，没有任何动作，只是简单的输出文件路径。
fzf默认是find命令来查找文件，你可以换成别的命令，或者直接给出数据源。

##### vim配置
`Plug junegunn/fzf.vim`  下载插件
`set rtp+=~/.fzf` 如果是homebrew安装，需要设置这句话

##### 执行命令
```
:Files      查询文件
:Buffers    查询buffers
:History    查询历史打开过的文件
```
> [ag](https://github.com/ggreer/the_silver_searcher)

![pic alt](http://o99eh3ii0.bkt.clouddn.com//16-9-16/19872916.jpg)
ag是一个命令行下的代码查询工具。 ag > ack > grep 。 查询速度非常快 。 可以和vim结合，查找的结果放在一个buffer中。
它会自动忽略git/hg/svn-ignore的忽略内容。

##### 安装
`sudo brew install the_silver_searcher`
`Plug 'rking/ag.vim'`

##### 与fzf结合
安装fzf后，执行 `:Ag someword `,其结果可以用fzf筛选。
![pic alt](http://o99eh3ii0.bkt.clouddn.com//16-9-16/74584269.jpg)


## 代码格式化

autoFomate + js-beautiful

## 括号补全



## 终端模拟

neoterm
vimshell



