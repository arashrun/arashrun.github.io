> 记录原生vim中的一些概念，不涉及外部插件等用法和配置
> 基本都是从vim的官方帮助文档中总结出来的有用的特性和概念(https://vimhelp.org/)



## 安装

1.  下载最新vim源码包。
2. ./configure --with-features=huge --enable-python3interp --enable-multibyte
3.  执行make && make install



<br>

## 基本概念：

<b>1. 文件，缓冲区，窗口，标签页</b>

文件：磁盘中的数据块

缓冲区：加载到内存中的文件

窗口：用于承载缓存区的容器，通过sp，vs等操作可以创建新窗口(对应于tmux中的panel的概念)

标签页：承载多个窗口的容器，通过tabnew创建




<br>

## vim脚本语法

### 变量类型：

-   全局变量	g:name
-   脚本范围局部变量    s:name 
-   vim预定义变量    v:name 
-   函数内局部变量：l:name 
-   函数参数：a:name 
-   缓冲区局部变量：b:name 
-   窗口局部变量：w:name 
-   标签页局部变量：t:name 

>   脚本范围的局部变量，相当于c语言中static关键字，定义之后，只在当前声明的文件范围有效。

<br>

### 变量定义：

```shell
let g:a = 123
let s:b = "adf"
```



### 取消变量定义：

```shell
unlet s:a
```

### 表达式
1 基本表达式
    数值
    字符串常量
    $name 环境变量
    &name 选项
    @reg  寄存器
2 算法表达式
    + - * / %
    "hello" . "world" =="helloworld" 字符串拼接
### 函数
call function_name(para1,para2) 	调用函数
let line = getline('.') 			调用函数作为表达式
:functions 							查看vim中内置的完整函数列表

1 定义函数
    function {name}({var1,{var2},...})
        {body}
    endfunction
    - 函数名必须以大写字母开始
    - a:var1 代表函数参数变量
    - 函数内部没有特殊标识的变量(g: a: s:)都是局部变量
    - 函数内部访问全局变量要加 g:
2 重定义函数
    function! name(var1,var2)
3 范围使用
    :10,30call funcname()
        function funcname() range 	定义
4 可变参数
    function Show(start,...)
    a:0 	可变参数个数
    a:1 	第一个可变参数
    a:2 	第二个可变参数
    a:000 	可变参数列表
5 删除函数
    delfunction xxx



## windows-gui版本基本配置

```
set rnu
syntax on
set guifont=source_code_pro:h10
set incsearch
set guioptions-=T
set guioptions-=r
set guioptions-=m
set encoding=utf-8
set fileencodings=utf-8,cp936,ucs-bom,shift-jis,gb18030,gbk,gb2312,utf-16le
set backspace=2
"set cursorline
set hlsearch
" stop cursor blink
set gcr=a:block-blinkon0
" the default leader word is '\'
" here use mapleader to remap the leader word
let mapleader=","
map <F5> :so $MYVIMRC<cr>
map <F6> :e $MYVIMRC<cr>
inoremap <c-k> <esc>:w<cr>
inoremap <c-i> <c-o>A
nnoremap <c-k> <esc>:w<cr>
nnoremap <esc> :noh<cr>
map M :%s/^M//g<cr>
vnoremap // y/<c-r>"<cr>

" switch windows
nnoremap <tab> :bn<cr>
nnoremap <space> :bp<cr>
nnoremap ,d :bp <bar> bd #<cr>
tnoremap <c-x> <c-\><c-n>
" switch tabs
nnoremap ,1 :tabprevious<cr>
nnoremap ,2 :tabNext<cr>
"quick scroll screen
nnoremap fj <c-d>
nnoremap fk <c-u>
nnoremap fh <c-f>
nnoremap fl <c-b>
nnoremap <M-l> <c-w>l
nnoremap <M-w> <c-w>w
nnoremap <M-h> <c-w>h
"let the c-l ==>  insert mode move the cursor to left
imap <c-l> <esc>la
inoremap <esc> <esc>:w<cr>

"""""""""""""""""""""""""""""""""
""""""""""quickfix"""""""""""""""
"""""""""""""""""""""""""""""""""
"open quickfix
nnoremap 'e :cw<cr>
"goto next item
nnoremap 'j :cn<cr>
"goto prev item
nnoremap 'k :cp<cr>
nnoremap ,s :botright terminal++rows=15<cr>

"""""""""""""""""""""""""""""""""
""""""""""markdown"""""""""
"""""""""""""""""""""""""""""""""
autocmd FileType markdown :iabbrev <buffer> bd ****<esc>hi
autocmd FileType markdown :iabbrev <buffer> uu <u style="color:red"></u><esc>3hi
autocmd FileType markdown :iabbrev <buffer> h1 #
autocmd FileType markdown :iabbrev <buffer> h2 ##
autocmd FileType markdown :iabbrev <buffer> h3 ###
autocmd FileType markdown :iabbrev <buffer> h4 ####
autocmd FileType markdown :iabbrev <buffer> h5 #####


```


## vim功能复杂版本配置

```bash
" check if has shortcut has defined
" :verbose map <shortcut>
set nu
syntax on
set encoding=utf-8
set fileformats=unix,dos
"set relativenumber
set cursorline
set incsearch
set ignorecase smartcase
set hlsearch
set showmatch
set wrap
set backspace=2
set scrolloff=5
"Bug in windows terminal 
"https://github.com/microsoft/terminal/issues/1637#issuecomment-663865934
set t_u7=
" add tab space
" ts 是tabstop的缩写，设TAB宽度为4个空格。
" softtabstop 表示在编辑模式的时候按退格键的时候退回缩进的长度，当使用
" expandtab 时特别有用。
" shiftwidth 表示每一级缩进的长度，一般设置成跟 softtabstop 一样。
" expandtab表示缩进用空格来表示，noexpandtab 则是用制表符表示一个缩进。
" autoindent自动缩进
set ts=4
set expandtab
set softtabstop=4
set shiftwidth=4
set autoindent

"""""""""""""""""""""""""""""""""
""""""""""通用配置相关"""""""""""
"""""""""""""""""""""""""""""""""
" the default leader word is '\'
" here use mapleader to remap the leader word
let mapleader=","
map <F5> :so $MYVIMRC<cr>
map <F6> :!ctags -R<cr>
imap <c-k> <esc>:w<cr>
nnoremap <esc> :noh<cr>
map M :%s/^M//g<cr>
vnoremap // y/<c-r>"<cr>
"""""""""""""""""""""""""""""""""
""""""""""窗口相关"""""""""""
"""""""""""""""""""""""""""""""""
" switch windows
map ` <c-w>
" switch tabs
map ,1 :tabprevious<cr>
map ,2 :tabNext<cr>
"quickfix
"open quickfix
map 'e :cw<cr>
"goto next item
map 'j :cn<cr>
"goto prev item
map 'k :cp<cr>
"quick scroll screen
map fj <c-d>
map fk <c-u>
map fh <c-f>
map fl <c-b>
"operat buffers
map 'm :bn<cr>
map 'n :bp<cr>
map 'd :**<cr>

"""""""""""""""""""""""""""""""""
""""""""""markdown快捷键"""""""""
"""""""""""""""""""""""""""""""""
autocmd FileType markdown imap <buffer> <c-q> # 
autocmd FileType markdown imap <buffer> <c-w> ## 
autocmd FileType markdown imap <buffer> <c-e> ### 
autocmd FileType markdown imap <buffer> <c-r> #### 
autocmd FileType markdown imap <buffer> <c-t> ##### 


"""""""""""""""""""""""""""""""""
""""""""""修改操作相关"""""""""""
"""""""""""""""""""""""""""""""""

map ,t :Defx -buffer-name=`'defx' . tabpagenr()`<cr>
map ,f <Plug>(coc-translator-p)

"let the c-l ==>  insert mode move the cursor to left
imap <c-l> <esc>la
" ctrl-enter
imap <NL> <esc>o

iab xtime <c-r>=strftime("%Y-%m-%d %H:%M:%S")<cr>
nmap ,ha :call self#AutoGetFileName()<CR>

call plug#begin('~/.vim/plugged')
Plug 'https://gitee.com/yaozhijin/vim-airline.git'
Plug 'https://gitee.com/daibao9922/coc.nvim.git',{'branch':'release'}
Plug 'https://gitee.com/zgpio/vim-airline-themes.git'
"Plug 'https://gitee.com/jxsylar/ctrlp.vim.git'
"Plug 'https://gitee.com/LKcode/vim-startify.git'
Plug 'https://gitee.com/yaozhijin/nerdcommenter.git'
if has('nvim')
  Plug 'https://gitee.com/yaozhijin/defx.nvim.git', { 'do': ':UpdateRemotePlugins' }
  Plug 'https://gitee.com/zgpio/defx-icons.git'
else
  Plug 'https://gitee.com/yaozhijin/defx.nvim.git'
  Plug 'https://gitee.com/zgpio/nvim-yarp.git'
  Plug 'https://gitee.com/zgpio/vim-hug-neovim-rpc.git'
  Plug 'https://gitee.com/zgpio/defx-icons.git'
endif
Plug 'https://gitee.com/yaozhijin/vim-floaterm.git'
"Plug 'https://gitee.com/arashrun/vim-markdown.git'
Plug 'octol/vim-cpp-enhanced-highlight'
Plug 'https://gitee.com/zgpio/LeaderF.git', { 'do': ':LeaderfInstallCExtension' }
Plug 'https://gitee.com/zgpio/ultisnips.git'

" colorscheme
Plug 'flrnd/candid.vim'
Plug 'morhetz/gruvbox'
Plug 'joshdick/onedark.vim.git'
Plug 'https://gitee.com/arashrun/vim-dogrun.git'
call plug#end()


"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""""""""""""""""""""ultisnips configured""""""""""""""""""""""""""""
let g:UltiSnipsExpandTrigger="<c-;>"
let g:UltiSnipsEditSplit="vertical"


"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""""""""""""""""""""leaderf configured""""""""""""""""""""""""""""
" map ctrl-p to find the file
let g:Lf_ShortcutF = '<C-P>'
noremap ;f :<C-U><C-R>=printf("Leaderf! rg -e %s ", expand("<cword>"))<CR>

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
""""""""""""""""""""""""vim floaterm configured"""""""""""""""""""""""
let g:floaterm_keymap_new = '<leader>w'
let g:floaterm_keymap_prev   = '<leader>q'
let g:floaterm_keymap_next   = '<leader>e'
let g:floaterm_keymap_toggle = '<leader>r'

hi FloatermBorder guibg=orange


"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
""""""""""""""""""""""""airline configured"""""""""""""""""""""""
"let g:airline_theme='molokai'
let g:airline_theme='bubblegum'
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#formatter = 'unique_tail'
let g:airline#extensions#tabline#buffer_nr_show = 1
"let g:airline#extensions#tabline#left_sep = '<'
"let g:airline#extensions#tabline#right_sep = '>'
"let g:airline#extensions#tabline#left_alt_sep = '##'
" unicode symbols

"if !exists('g:airline_symbols')
"    let g:airline_symbols = {}
"endif
"let g:airline_left_sep = '▶'
"let g:airline_left_alt_sep = '❯'
"let g:airline_right_sep = '◀'
"let g:airline_right_alt_sep = '❮'
"let g:airline_symbols.linenr = '¶'
"let g:airline_symbols.branch = '⎇'

"let g:airline_symbols.crypt = ''
"let g:airline_symbols.linenr = '☰'
"let g:airline_symbols.linenr = '␊'
"let g:airline_symbols.linenr = '␤'
"let g:airline_symbols.maxlinenr = ''
"let g:airline_symbols.maxlinenr = '㏑'
"let g:airline_symbols.paste = 'ρ'
"let g:airline_symbols.paste = 'Þ'
"let g:airline_symbols.paste = '∥'
"let g:airline_symbols.spell = 'Ꞩ'
"let g:airline_symbols.notexists = '∄'
"let g:airline_symbols.whitespace = 'Ξ'

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
""""""""""""""""""""""""ctrlp configured"""""""""""""""""""""""
" https://vimjc.com/vim-ctrlp-plugin.html
" :help ctrlp.txt 获取ctrlp的官方说明文档
"ctrlp显示在上头
"let g:ctrlp_match_window = 'top,order:ttb,min:1,max:10,results:20'
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
""""""""""""""""""""""""coc configured""""""""""""""""""""""""""""""""""""
"CocInstall coc-marketplace
"CocList marketplace
"coc-translator, help-translator

let g:coc_global_extensions = [ 'coc-json', 
                            \   'coc-clangd',
                            \   'coc-cmake',
                            \   'coc-pyright' 
                            \   ]

inoremap <silent><expr> <TAB>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()
inoremap <expr><S-TAB> pumvisible() ? "\<C-p>" : "\<C-h>"

function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~# '\s'
endfunction

inoremap <silent><expr> <cr> pumvisible() ? coc#_select_confirm()
                              \: "\<C-g>u\<CR>\<c-r>=coc#on_enter()\<CR>"

if has('nvim')
  inoremap <silent><expr> <c-space> coc#refresh()
else
  inoremap <silent><expr> <c-@> coc#refresh()
endif

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""""""""""""""""""""""""commenter configured"""""""""""""""""""""""""""""""""
"<leader>cc   加注释
"<leader>cu   解开注释
"
"<leader>c<space>  加上/解开注释, 智能判断
"
"注释左对齐
let g:NERDDefaultAlign = 'left'
"ctrl+/
"map <c-_> <leader>c<space>
map <c-c> <leader>c<space>
"<leader>cy   先复制, 再注解(p可以进行黏贴)


""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""""""""""""""""""""""""startify configured"""""""""""""""""""""""""""""""""
"起始页显示的列表长度
"let g:startify_files_number = 20
"自动加载session
"let g:startify_session_autoload = 1



""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""""""""""""""""""""""""defx configured"""""""""""""""""""""""""""""""""
" 动态更新目录

call defx#custom#column('icon', {
      \ 'directory_icon': '▸',
      \ 'opened_icon': '▾',
      \ 'root_icon': ' ',
      \ })

call defx#custom#column('filename', {
      \ 'min_width': 40,
      \ 'max_width': 40,
      \ })

call defx#custom#column('mark', {
      \ 'readonly_icon': '✗',
      \ 'selected_icon': '✓',
      \ })

call defx#custom#option('_', {
      \ 'winwidth': 30,
      \ 'split': 'vertical',
      \ 'direction': 'topleft',
      \ 'show_ignored_files': 0,
      \ 'buffer_name': '',
      \ 'toggle': 1,
      \ 'resume': 1
      \ })

autocmd BufWritePost * call defx#redraw()
"autocmd BufEnter * call s:open_defx()

autocmd FileType defx call s:defx_my_settings()
function! s:defx_my_settings() abort
  " Define mappings
  nnoremap <silent><buffer><expr> <CR>
  \ defx#do_action('open')
  " open the file in the right buffer
  nnoremap <silent><buffer><expr> <CR> 
  \ defx#do_action('drop')
  " 上一级目录
  nnoremap <silent><buffer><expr> h
  \ defx#do_action('cd', ['..'])
  " 打开目录
  nnoremap <silent><buffer><expr> o
  \ defx#do_action('open_tree', 'toggle')
  " 复制文件名
  nnoremap <silent><buffer><expr> yy
  \ defx#do_action('yank_path')
  " 开启、关闭忽视隐藏文件
  nnoremap <silent><buffer><expr> .
  \ defx#do_action('toggle_ignored_files')
  " 跳转到光标所在位置的上一级目录处，一般配合o来使用，快速跳转折叠
  nnoremap <silent><buffer><expr> P 
  \ defx#do_action('search', fnamemodify(defx#get_candidate().action__path, ':h'))
  " 修改当前工作目录为进入的目录
  nnoremap <silent><buffer><expr> cd
  \ defx#do_action('change_vim_cwd')
  nnoremap <silent><buffer><expr> c
  \ defx#do_action('copy')
  nnoremap <silent><buffer><expr> m
  \ defx#do_action('move')
  nnoremap <silent><buffer><expr> p
  \ defx#do_action('paste')
  nnoremap <silent><buffer><expr> d
  \ defx#do_action('remove')


  nnoremap <silent><buffer><expr> l
  \ defx#do_action('open')
  nnoremap <silent><buffer><expr> E
  \ defx#do_action('open', 'vsplit')
  "nnoremap <silent><buffer><expr> P
  "\ defx#do_action('preview')
  nnoremap <silent><buffer><expr> K
  \ defx#do_action('new_directory')
  nnoremap <silent><buffer><expr> N
  \ defx#do_action('new_file')
  nnoremap <silent><buffer><expr> M
  \ defx#do_action('new_multiple_files')
  nnoremap <silent><buffer><expr> C
  \ defx#do_action('toggle_columns',
  \                'mark:indent:icon:filename:type:size:time')
  nnoremap <silent><buffer><expr> S
  \ defx#do_action('toggle_sort', 'time')
  nnoremap <silent><buffer><expr> r
  \ defx#do_action('rename')
  nnoremap <silent><buffer><expr> !
  \ defx#do_action('execute_command')
  nnoremap <silent><buffer><expr> x
  \ defx#do_action('execute_system')
  nnoremap <silent><buffer><expr> ;
  \ defx#do_action('repeat')
  nnoremap <silent><buffer><expr> ~
  \ defx#do_action('cd')
  nnoremap <silent><buffer><expr> q
  \ defx#do_action('quit')
  nnoremap <silent><buffer><expr> <Space>
  \ defx#do_action('toggle_select') . 'j'
  nnoremap <silent><buffer><expr> *
  \ defx#do_action('toggle_select_all')
  nnoremap <silent><buffer><expr> j
  \ line('.') == line('$') ? 'gg' : 'j'
  nnoremap <silent><buffer><expr> k
  \ line('.') == 1 ? 'G' : 'k'
  nnoremap <silent><buffer><expr> <C-l>
  \ defx#do_action('redraw')
  nnoremap <silent><buffer><expr> <C-g>
  \ defx#do_action('print')
endfunction


""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""""""""""""""""""""""""defx icons configured"""""""""""""""""""""""""""""""""
let g:defx_icons_term_colors = {
\ 'red': 2
\ }

let g:defx_icons_enable_syntax_highlight = 1
let g:defx_icons_column_length = 1
let g:defx_icons_directory_icon = ''
let g:defx_icons_mark_icon = '*'
let g:defx_icons_copy_icon = ''
let g:defx_icons_link_icon = ''
let g:defx_icons_move_icon = ''
let g:defx_icons_parent_icon = ''
let g:defx_icons_default_icon = ''
let g:defx_icons_directory_symlink_icon = ''
" Options below are applicable only when using "tree" feature
let g:defx_icons_root_opened_tree_icon = ''
let g:defx_icons_nested_opened_tree_icon = ''
let g:defx_icons_nested_closed_tree_icon = ''


""""""""""""""""""""""""""""""""""""""""colorscheme choose """"""""""""""""""""""""""""""""""""""""

set background=dark
"set background=light

"gruvbox need background = dark
autocmd vimenter * ++nested colorscheme gruvbox


"colorscheme monokai-bold
"colorscheme candid
"colorscheme onedark
"colorscheme dogrun
set mouse=a

```


## vim 插件

### vim8+本地插件管理

https://www.danielfranklin.id.au/vim/vim-8-package-management/

### 常用插件















































<br>
<h2>常见问题</h2>

<b>1. win上dos的文件到unix上就会多出一个^M符号</b>
<br>
<b>reason:</b> win上是用两个字符代表换行(\r\n)，而linux采用简洁原则换行符采用(\n)。因此导致win上的文件导入到unix等类平台时会多出一个(\r)字符，而该字符在unxi上就是显示为^M(ctrl-v)
<b>solution:</b> :%s/\r//g


## TODOLIST
1. v:在当前文件查找选择的可视化部分
2. i:如何创建代码片段
