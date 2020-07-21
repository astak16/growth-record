## `Vim`

`Vim` = `Vi` + `IMproved`

- 多级撤销
- 语法高亮和自动补全
- 支持多种插件
- 通过网络协议(`HTTP/SSH`)编辑文件
- 多文件编辑
- `Vim`可以编辑压缩格式文件(`gzip`、`zip`)

## `Vimrc`

### `vimrc`概念

- `rc` = `run command`，运行命令的意思
- 系统级`vimrc`和用户级`vimrc`
  - 系统级对所有用户都生效
  - 用户级是对象各个用户
- 每一行作为一个命令执行

### `vimrc`使用

```vim
:h vimrc    // 帮助文档
:version    // 版本相关信息
:e ~/.vimrc   // vim中 快速打开 vimrc 中
```

命令行中使用

```vim
:set number     // 设置行号
:set nonumber   // 取消行号
:set number?    // 查看返回值
:set hls        // 搜索高亮，hlsearch 可以简写成 hls
```

配置文件中使用

`set`篇：

```vim
"                     // 单个双引号是注释
set history=1000      // 命令行中历史记录设置为 1000 条
set ruler             // 设置右下角是否显示光标行列号
set hlsearch          // 搜索高亮，输入结束后，回车高亮
set incsearch         // 搜索中高亮
set ignorecase        // 忽略查找选项的大小写
set number            // 设置行号
set autoindent        // 复制当前行的缩进到下一行
set smartindent       // 自动识别语言中的花括号，进行缩进
set expandtab         // 使用空格代替 tab
set bg=light          // 设置背景颜色
```

`map`篇：设置快捷键的

```vim
map <F3> i<ul><CR><Space><Space><li></li><CR><Esc>I</ul><Esc>kcit   // 解释1
let mapleader=”,“   // 赋值
map <leader>w :w!<cr>   // 解释2
```

解释 1：

```vim
<F3>    // 快捷键 F3
i       // 进入插入模式
<ul>    // 内容
<CR>    // 回车的意思
<Space> // 空格的意思
<Esc>   // 普通模式
I       // 到当前行的首字母，并进入编辑模式
```

解释 2：

```vim
<leader>    // 上面将 mapleader 赋值为 ,
<leader>w   // ,w
<leader>w :w!<cr>   // 用 ,w 代替 :w!
```

## 搜索和替换

语法：`:[range]s[ubstitute]/{pattern}/{string}/[flags]`
- `range`表示范围，比如：`10,20`，表示`10-20`行，`%`表示全部
- `pattern`是要替换的模式
- `string`是替换后的文本
- `flags`
  - `g(global)`表示全局范围内执行
  - `c(confirm)`表示确认，可以确认或者拒绝修改
  - `n(number)`报告匹配到的次数而不替换，可以用来查询匹配次数

`eg`：
 - `:% s/self/this/g`：把全局`self`替换成`this`
 - `:1,6 s/self/this/g`：把`1-6`行的`self`替换成`this`
 - `:1,6 s/self//n`：搜索`1-6`行的`self`有多少个
 - `:% s/\<quack\>/jiao/g`：使用正则只匹配`quack`这个单词。
 
## 多文件操作
- `Buffer`，查看文档`:h buffer`
  - `Buffer`是指打开的一个文件的内存缓冲区
    - `Vim`打开一个文件后会加载文件内容到缓冲区
    - 之后的修改都是针对内存中的缓冲区，并不会直接保存到文件
    - 直到我们执行`:w`的时候才会把修改的内容写入到文件里
  - `Buffer`之间切换
    - 使用`:ls`会列举当前缓冲区，然后使用`:b n`跳转到底`n`个缓冲区
    - `:bpre`、`:bnext`、`:bfirst`、`:blast`
    - 或者用`:b buffer_name`加上`tab`补全来跳转
- `Window`，查看文档`:h window`
  - 窗口是`Buffer`可视化的分割区域
    - 一个缓冲区可以分割多个窗口，每个窗口也可以打开不同缓冲区
    - `<Ctrl+w>s`水平分割，`<Ctrl+w>v`垂直分割，在命令行模式下可以使用`:sp`水平分割，`:vs`垂直分割
    - 每个窗口可以继续被无限分割（看你屏幕是否够大）
  - 窗口之间切换
    - `<Ctrl+w>w`：在窗口见循环切换
    - `<Ctrl+w>h`：切换到左边的窗口
    - `<Ctrl+w>j`：切换到下边的窗口
    - `<Ctrl+w>k`：切换到上边的窗口
    - `<Ctrl+w>l`：切换到右边的窗口
  - 改变窗口的大小`:h window-resize`查看帮助文档
    - `<Ctrl+w>=`：使所有窗口等宽、等高
    - `<Ctrl+w>_`：最大化活动窗口的高度
    - `<Ctrl+w>|`：最大化活动窗口的宽度
    - `[N]<Ctrl+w>_`：把活动窗口的高度设为`[N]`行
    - `[N]<Ctrl+w>|`：把活动窗口的宽度设为`[N]`行
- `Tab`，查看文档`:h tabpage`
  - `Tab`可以组织窗口为一个工作区，它是一系列窗口的容器
    - `Vim`的`Tab`和其他的编辑器不太一样，可以想象成`Linux`的虚拟桌面
    - 比如一个`Tab`全用来编辑`Python`文件，一个`Tab`全用来编辑`HTML`文件
    - 相比窗口，`Tab`一般用的比较少，`Tab`太多管理起来也比较麻烦
  - 操作
    - `tab[edit] {filename}`：在新标签页中打开`{filename}`
    - `<Ctrl+w>T`：把当前窗口移到一个新标签页
    - `tab[close]`：关闭当前标签页及其中的所有窗口
    - `tab[only]`：只保留活动标签页，关闭所有其他标签页
    - `:tab[next] {N}`---`{N}gt`：切换到编号为`{N}`的标签页
    - `:tab[next]`---`gt`：切换到下一标签页
    - `:tab[previous]`---`gT`：切换到上一标签页

`:e 1.text`在命令行模式下打开文件
## 模式

`4`种模式：
- 普通模式
  - 默认的
  - 移查删改
- 可视模式
  - 对一整块区域操作
  - 按`v`，左下角有一个`VISUAL`的提示
- 插入模式
  - 添加文本
  - 按`i`，左下角有一个`INSERT`的提示
- 命令模式
  - 和普通模式类似
  - 按`:`

- `i`：`insert` 在字符前插入
- `I`：`insert` befort line 在当前行首插入
- `a`：`append` 在字符后插入
- `A`：`inster after line` 在当前行尾插入
- `o`：`open a line below` 在当前行下一行插入
- `O`：`open a line above` 在当前行上一行插入

`e!`：不保存，重新加载文件



## 学习资源
《Practical vim》，中文版《vim 使用技巧》

《笨办法学 vimscript》