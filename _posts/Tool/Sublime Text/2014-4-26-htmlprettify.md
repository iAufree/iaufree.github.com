---
layout: post
title: 代码排版及取色
date:   2014-04-26 17:08:00
categories: 工具
tags: [sublime]
---


###代码排版

今天想试用一下Sublime Text的一个排版插件,叫`HTML-CSS-JS Prettify`,所以打算用`bootstrap`的压缩文件练练手.

这是排版前的CSS:

<img src="\public\upload\images\html-prettify.jpg" alt="排版前" class="shadow-img" title="排版前">

可以看到`bootstrap`把所以的CSS都压缩到了第8行,在排版过程中需要用到2样东西,`Reindent`和`HTML-CSS-JS Prettify`,前者只能对代码的进行缩进处理,并不能对代码进行换行处理,因此还是需要后者的帮忙,接下来开始配置:

1.设置Reindent,可以直接用`edit>line>reindent`,如果要使用快捷键,在`Preferences → Key Bindings – User`中加入

	{"keys": ["super+shift+r"], "command": "reindent" , "args": {"single_line": false}}

`key`后面是需要设置的快捷键,具体看[这里](https://joshbetz.com/2012/09/reindent-text-in-sublime),配置完以后选中要排版缩进的代码,按快捷键就能完成排版了.

2.使用`HTML-CSS-JS Prettify`,首先确保电脑已经安装了[node.js](http://nodejs.org/),安装教程在[这里](http://blueashes.com/2011/web-development/install-nodejs-on-windows/).

安装过程中如果出现`无法打开安装程序包`,可以试着直接下载[镜像](http://dist.u.qiniudn.com/v0.10.26/node-v0.10.26-x86.msi).

安装完之后,就可以安装[HTML-CSS-JS Prettify](https://sublime.wbond.net/packages/HTML-CSS-JS%20Prettify)了,按快捷键`Ctrl+Shift+P` or `Cmd+Shift+P` in Linux/Windows/OS X,输入`ip` → 回车 → 输入 `htmlprettify` → 安装.

安装之后,点击 View → Show Console, 然后输入 `view.run_command("htmlprettify")`.

运行如果得到`Node. Js was not found in the default path. Both Please specify the location. `或`command not found!`.

表示node路径还没设置好,按快捷键 `Ctrl+Shift+P` or `Cmd+Shift+P` in Linux/Windows/OS X,输入`htmlprettify`, 选择 `Set node Path`,设置下路径就好了.

我把装在了`C:\Program Files (x86)\nodejs`,所以得把路径设置为`C:\\Program Files (x86)\\nodejs\\node`.

同时也可以自己设置排版的格式.插件具体说明参见[这里](https://github.com/victorporof/Sublime-HTMLPrettify).

先前的代码经过排版之后变成这样:

<img src="\public\upload\images\html-prettify2.jpg" alt="排版后" class="shadow-img" title="排版后">

如果想压缩代码可以使用 ctrl + L 快捷键.

WoW!的确是神器!

###修复Color Picker

前段时间去安装了一个取色插件,叫[Color Picker](https://github.com/weslly/ColorPicker/),安装完之后,取色超方便,一个快捷键就能调出来取色板,最近快捷键出了点问题,怎么按都没反应,于是去Color Picker的提问区问了一下,最后[LOQUILLO](https://github.com/LOQUILLO)给出了[解决方案](https://github.com/weslly/ColorPicker/issues/43#issuecomment-41391919),重新设置一下该插件的快捷键就行了.

