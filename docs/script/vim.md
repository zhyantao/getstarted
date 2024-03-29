# Vim

## 移动

```{code-block} bash
h j k l     # 左 下 右 上

gg          # 光标移动到文件开头
G           # 光标移动到文件末尾
0           # 光标移动到行首
$           # 光标移动到行尾
23G         # 光标跳转到第23行
:23         # 跳转到第 23 行
```

## 插入

```{code-block} bash
i           # 在光标前插入一个字符
I           # 在行首插入一个字符
a           # 在光标后插入一个字符
A           # 在行尾插入一个字符
o           # 向下新开辟一行，插入行首
O           # 向上新开辟一行，插入行首
s           # 删除光标所在的字符，并进入插入状态
S           # 删除光标所在的行，并进入插入状态
```

## 复制、粘贴

```{code-block} bash
yy          # 复制当前行
10yy        # 复制 10 行

p           # 在光标所在位置向下开辟一行，粘贴
P           # 在光标所在位置向上开辟一行，粘贴

set paste   # 防止粘贴时，格式错乱
```

## 剪切、删除

```{code-block} bash
x           # 删除光标后一个字符
X           # 删除光标前一个字符
dw          # 删除光标开始位置的单词
d0          # 删除光标前本行文本的所有内容，不包含光标所在的字符
D           # 删除光标后本行所有的内容，包含光标所在的字符
dd          # 删除光标所在的行
n dd        # 删除光标后面所有的行，包含光标所在的行

```

## 撤销

```{code-block} bash
u           # 一步撤销，可多次使用
Ctrl + r    # 反撤销
```

## 查找

```{code-block} bash
/PATTERN    # 从光标所在位置向下查找
?PATTERN    # 从光标所在位置向上查找
    Enter   # 输入完毕
    n       # 向下查找下一个
    N       # 向上查找下一个
    #       # 查找光标当前指向的字符串
```

## 替换

```{code-block} bash
r                           # 替换当前字符
:s/PATTERN/toString         # 将当前行第一次出现的 PATTERN 替换为 toString
:s/PATTERN/toString/g       # 将当前行出现的 PATTERN 全部替换为 toString
:%s/PATTERN/toString        # 将所有行第一次出现的 PATTERN 替换为 toString
:%s/PATTERN/toString/g      # 将当前行出现的 PATTERN 全部替换为 toString
:2,3s/PATTERN/toString      # 将[2,3]行第一次出现的 PATTERN 替换为 toString
:2,3s/PATTERN/toString/g    # 将[2,3]行第一次出现的 PATTERN 替换为 toString
```

## 格式化文本

```{code-block} bash
>>          # 文本右移一个 Tab 大小
<<          # 文本左移一个 Tab 大小
gg=G        # 整理代码，使其符合标准格式
```

## 全选

```{code-block} bash
ggVG        # 全部高亮
```

## 窗口

```{code-block} bash
Ctrl + S    # 冻结窗口
Ctrl + q    # 解冻窗口
```

## 记住上次打开的位置

```{code-block} bash
" Uncomment the following to have bash jump to the last position when
" reopening a file
if has("autocmd")
  au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
endif
```

## 配置 Vim 插件

Linux 默认安装的 VIM 版本是 7.x，如果要使用 YouCompelteMe，需要先升级到 8.x。

为了更方便地管理我们的 VIM 插件，推荐使用 Vundle。具体步骤如下：

```{code-block} bash
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

新建或打开 `~/.vimrc`，然后再文件中添加如下内容：

```{code-block} text
" General settings
set nu                  " show line numbers
set tabstop     =4      " Width of tab character
set softtabstop =4      " Fine tunes the amount of white space to be added
set shiftwidth  =4      " Determines the amount of whitespace to add in normal mode
"set expandtab          " When this option is enabled, vi will use spaces instead of tabs

set nocompatible        " be iMproved, required
filetype off            " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'
Plugin 'vim-scripts/Auto-Pairs'             " Autocomplete parens/quotes/brackets

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
```

打开 `vim`，使用命令 `:PluginInstall` 安装插件。

如果要删除某些不需要的插件，可以参考上面配置文件中的 Brief help 部分。首先编辑 `.vimrc`
将不需要的 Plugin 删掉，先运行 `:PluginUpdate!` 然后再运行 `:PluginClean!` 即可。
