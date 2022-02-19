---
layout: post
title: vim配置
subtitle: 
tags: [环境配置,vim]
---



```
.vimrc

"==================================================================
"1,Workbench
"2,FileSetting
"3,TextEdit
"4,Keymap
"5,Appearance
"==================================================================

"==================================================================
"1,Workbench
"==================================================================
set noundofile " 不生成撤销文件，FileName.un.~
set nobackup " 不生成备份文件，FileName.~
set noswapfile 
setlocal noswapfile " 不要生成swap文件
set autoread " 设置当文件在外部被修改，自动更新该文件
set nocompatible " 关闭 vi 兼容模式
set clipboard+=unnamed " 共享剪切板
set autochdir " 自动切换当前目录为当前文件所在的目录
filetype on " 检测文件类型
filetype plugin indent on " 开启插件
set backupcopy=yes " 设置备份时的行为为覆盖
set noerrorbells " 关闭错误信息响铃
set novisualbell " 关闭使用可视响铃代替呼叫
set t_vb= " 置空错误铃声的终端代码
set magic " 设置魔术
set hidden " 允许在有未保存的修改时切换缓冲区，此时的修改由 vim 负责保存
set cmdheight=1 " 设定命令行的行数为 1
winpos 700 100 " 设置窗口位置
" set lines=34 columns=226 " 设置窗口大小
" set go= " 不要图形按钮
set guioptions-=T " 隐藏工具栏
set guioptions-=r " 隐藏右侧滚动条
set guioptions-=R 
set guioptions-=l " 隐藏左侧滚动条
set guioptions-=L 
" set guioptions-=m " 隐藏菜单栏
"""""""""" 语言设置
" set langmenu=zh_CN.UTF-8
set helplang=cn
"""""""""" vim提示信息乱码解决
" language message zh_CN,utf-8
"""""""""" 状态栏设置
set laststatus=2 " 显示状态栏 (默认值为 1, 无法显示状态栏)
set statusline=\ %<%F[%1*%M%*%n%R%H]%=\ %y\ %0(%{&fileformat}\ %{&encoding}\ Ln\ %l,\ Col\ %c/%L%) " 设置在状态行显示的信息
set ruler "在编辑过程中，在右下角显示光标位置的状态行


"==================================================================
"2,FileSetting
"==================================================================
"""""""""" 设置文件的代码形式 utf-8
set encoding=utf-8
set termencoding=utf-8
set fileencoding=utf-8
set fileencodings=utf-8,ucs-bom,gbk,gb2312
"""""""""" vim的菜单乱码解决
if(has("win32") || has("win95") || has("win64") || has("win7") || has("win10"))
  source $VIMRUNTIME/delmenu.vim
  source $VIMRUNTIME/menu.vim
endif
""""""""""  新建.c,.h,.sh,.java文件，自动插入文件头
autocmd BufNewFile *.cpp,*.[ch],*.sh,*.java exec ":call SetTitle()"
" 定义函数SetTitle，自动插入文件头
func SetTitle()
		"如果文件类型为.sh文件
		if &filetype == 'sh'
				call setline(1,"\#########################################################################")
				call append(line("."), "\# File Name: ".expand("%"))
				call append(line(".")+1, "\# Author: mkDlufop")
				call append(line(".")+2, "\# mail: mkDlufopqq.com")
				call append(line(".")+3, "\# Created Time: ".strftime("%c"))
				call append(line(".")+4, "\#########################################################################")
				call append(line(".")+5, "\#!/bin/bash")
				call append(line(".")+6, "")
		else
				call setline(1, "/*************************************************************************")
				call append(line("."), "    > File Name: ".expand("%"))
				call append(line(".")+1, "    > Author: mkDlufop")
				call append(line(".")+2, "    > Mail: mkDlufop@qq.com ")
				call append(line(".")+3, "    > Created Time: ".strftime("%c"))
				call append(line(".")+4, " ************************************************************************/")
				call append(line(".")+5, "")
		endif
		if &filetype == 'cpp'
				call append(line(".")+6, "#include<iostream>")
				call append(line(".")+7, "using namespace std;")
				call append(line(".")+8, "")
		endif
		if &filetype == 'c'
				call append(line(".")+6, "#include<stdio.h>")
				call append(line(".")+7, "")
		endif
		"新建文件后，自动定位到文件末尾
		autocmd BufNewFile * normal G
endfunc


"==================================================================
"3,TextEdit
"==================================================================
set nu " 显示行号
set rnu " 显示相对行号
set cursorline " 突出显示当前行
set smartindent " 开启新行时使用智能自动缩进
syntax enable
syntax on " 自动语法高亮
set showmatch " 插入括号时，短暂地跳转到匹配的对应括号
set matchtime=2 " 短暂跳转到匹配括号的时间
set smartindent "智能对齐
set autoindent "设置自动对齐
set bufhidden=hide " 当buffer被丢弃的时候隐藏它
"""""""""" fonts
set guifont=Consolas:h11 "设置字体为Monaco,大小为10
"""""""""" 查找/替换相关的设置
set incsearch " 输入搜索内容时就显示搜索结果
set hlsearch " 搜索时高亮显示被找到的文本
set ignorecase smartcase " 搜索时忽略大小写，但在有一个或以上大写字母时仍保持对大小写敏感
"""""""""" 折叠
set foldenable " 允许折叠
set foldmethod=syntax " 设置语法折叠
set foldcolumn=0 " 设置折叠区域的宽度
setlocal foldlevel=1 " 设置折叠层数为 1
nnoremap <space> @=((foldclosed(line('.')) < 0) ? 'zc' : 'zo')<CR> " 用空格键来开关折叠
"""""""""" Tab及空格等设置
set tabstop=4 " 设定 tab 长度为 4
set softtabstop=4 " 使得按退格键时可以一次删掉 4 个空格
set noexpandtab " 不要用空格代替制表符
set shiftwidth=4 " 设定 << 和 >> 命令移动时的宽度为 4
set smarttab " 在行和段开始处使用制表符
set backspace=indent,eol,start " 不设定在插入状态无法用退格键和 Delete 键删除回车符


"==================================================================
"4,Keymap
"==================================================================
"""""""""" ctrl + a
map <C-A> ggVGY
map! <C-A> <Esc>ggVGY
map <F12> gg=G
"""""""""" ctrl + c
vmap <C-c> "+y
"""""""""" 将ESC映射为两次j
inoremap jj <Esc>
"""""""""" 去空行
nnoremap <F2> :g/^\s*$/d<CR>
"""""""""" 比较文件
nnoremap <C-F2> :vert diffsplit
"""""""""" C，C++ 按F5编译运行
map <F5> :call CompileRunGcc()<CR>

func! CompileRunGcc()
    exec "w"
    if &filetype == 'c'
        exec "!g++ % -o %<"
        exec "! ./%<"
    elseif &filetype == 'cpp'
        exec "!g++ % -o %<"
        exec "! ./%<"
    elseif &filetype == 'java'
        exec "!javac %"
        exec "!java %<"
    elseif &filetype == 'sh'
        :!./%
    endif
endfunc
"""""""""" C,C++的调试
map <F8> :call Rungdb()<CR>

func! Rungdb()
    exec "w"
    exec "!g++ % -g -o %<"
    exec "!gdb ./%<"
endfunc


"==================================================================
"5,Appearance
"==================================================================
" colorscheme evening " 设定配色方案
" color asmanian2 " 设置背景主题
" set background=dark
"




```