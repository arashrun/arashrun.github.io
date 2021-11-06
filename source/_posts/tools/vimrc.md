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


" the default leader word is '\'
" here use mapleader to remap the leader word
let mapleader=","
imap <c-k> <esc>:w<cr>
nnoremap <esc> :noh<cr>
map 'w <c-w>w
map 'n :bn<cr>
map 'N :bp<cr>
map 'd :bd<cr>
map 'v :bp<cr>
map <F5> :so $MYVIMRC<cr>
map <F6> :!ctags -R<cr>
"quick scroll screen
map fj <c-d>
map fk <c-u>
map fh <c-f>
map fl <c-b>

map <leader>t :Defx -buffer-name=`'defx' . tabpagenr()`<cr>
map <leader>f <Plug>(coc-translator-p)
map M :%s/^M//g<cr>

map <leader>1 :tabprevious<cr>
map <leader>2 :tabNext<cr>

" `j = c-w-j
" `k = c-w-k
" switch windows
map ` <c-w>

"quickfix
"open quickfix
map 'e :cw<cr>
"goto next item
map 'j :cn<cr>
"goto prev item
map 'k :cp<cr>

"cscope remapping
map 'c <c-\>c
map 'g <c-\>g
map 's <c-\>s
map 'f <c-\>f

"let the c-l ==>  insert mode move the cursor to left
imap <c-l> <esc>la
" ctrl-enter
imap <NL> <esc>o

iab xtime <c-r>=strftime("%Y-%m-%d %H:%M:%S")<cr>


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


""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""""""""""""""""""""""""cscope configured"""""""""""""""""""""""""""""""""
set cscopequickfix=s-,c-,d-,i-,t-,e-,a-
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" CSCOPE settings for vim           
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"
" This file contains some boilerplate settings for vim's cscope interface,
" plus some keyboard mappings that I've found useful.
"
" USAGE: 
" -- vim 6:     Stick this file in your ~/.vim/plugin directory (or in a
"               'plugin' directory in some other directory that is in your
"               'runtimepath'.
"
" -- vim 5:     Stick this file somewhere and 'source cscope.vim' it from
"               your ~/.vimrc file (or cut and paste it into your .vimrc).
"
" NOTE: 
" These key maps use multiple keystrokes (2 or 3 keys).  If you find that vim
" keeps timing you out before you can complete them, try changing your timeout
" settings, as explained below.
"
" Happy cscoping,
"
" Jason Duell       jduell@alumni.princeton.edu     2002/3/7
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


" This tests to see if vim was configured with the '--enable-cscope' option
" when it was compiled.  If it wasn't, time to recompile vim... 
"if has("cscope")

    """"""""""""" Standard cscope/vim boilerplate

    " use both cscope and ctag for 'ctrl-]', ':ta', and 'vim -t'
    "set cscopetag

    " check cscope for definition of a symbol before checking ctags: set to 1
    " if you want the reverse search order.
    "set csto=0

    " add any cscope database in current directory
    "if filereadable("cscope.out")
    "    cs add cscope.out  
    " else add the database pointed to by environment variable 
    "elseif $CSCOPE_DB != ""
    "    cs add $CSCOPE_DB
    "endif

    " show msg when any other cscope db added
    "set cscopeverbose  


    """"""""""""" My cscope/vim key mappings
    "
    " The following maps all invoke one of the following cscope search types:
    "
    "   's'   symbol: find all references to the token under cursor
    "   'g'   global: find global definition(s) of the token under cursor
    "   'c'   calls:  find all calls to the function name under cursor
    "   't'   text:   find all instances of the text under cursor
    "   'e'   egrep:  egrep search for the word under cursor
    "   'f'   file:   open the filename under cursor
    "   'i'   includes: find files that include the filename under cursor
    "   'd'   called: find functions that function under cursor calls
    "
    " Below are three sets of the maps: one set that just jumps to your
    " search result, one that splits the existing vim window horizontally and
    " diplays your search result in the new window, and one that does the same
    " thing, but does a vertical split instead (vim 6 only).
    "
    " I've used CTRL-\ and CTRL-@ as the starting keys for these maps, as it's
    " unlikely that you need their default mappings (CTRL-\'s default use is
    " as part of CTRL-\ CTRL-N typemap, which basically just does the same
    " thing as hitting 'escape': CTRL-@ doesn't seem to have any default use).
    " If you don't like using 'CTRL-@' or CTRL-\, , you can change some or all
    " of these maps to use other keys.  One likely candidate is 'CTRL-_'
    " (which also maps to CTRL-/, which is easier to type).  By default it is
    " used to switch between Hebrew and English keyboard mode.
    "
    " All of the maps involving the <cfile> macro use '^<cfile>$': this is so
    " that searches over '#include <time.h>" return only references to
    " 'time.h', and not 'sys/time.h', etc. (by default cscope will return all
    " files that contain 'time.h' as part of their name).


    " To do the first type of search, hit 'CTRL-\', followed by one of the
    " cscope search types above (s,g,c,t,e,f,i,d).  The result of your cscope
    " search will be displayed in the current window.  You can use CTRL-T to
    " go back to where you were before the search.  
    "

    "nmap <C-\>s :cs find s <C-R>=expand("<cword>")<CR><CR>	
    "nmap <C-\>g :cs find g <C-R>=expand("<cword>")<CR><CR>	
    "nmap <C-\>c :cs find c <C-R>=expand("<cword>")<CR><CR>	
    "nmap <C-\>t :cs find t <C-R>=expand("<cword>")<CR><CR>	
    "nmap <C-\>e :cs find e <C-R>=expand("<cword>")<CR><CR>	
    "nmap <C-\>f :cs find f <C-R>=expand("<cfile>")<CR><CR>	
    "nmap <C-\>i :cs find i ^<C-R>=expand("<cfile>")<CR>$<CR>
    "nmap <C-\>d :cs find d <C-R>=expand("<cword>")<CR><CR>	


    " Using 'CTRL-spacebar' (intepreted as CTRL-@ by vim) then a search type
    " makes the vim window split horizontally, with search result displayed in
    " the new window.
    "
    " (Note: earlier versions of vim may not have the :scs command, but it
    " can be simulated roughly via:
    "    nmap <C-@>s <C-W><C-S> :cs find s <C-R>=expand("<cword>")<CR><CR>	

    "nmap <C-@>s :scs find s <C-R>=expand("<cword>")<CR><CR>	
    "nmap <C-@>g :scs find g <C-R>=expand("<cword>")<CR><CR>	
    "nmap <C-@>c :scs find c <C-R>=expand("<cword>")<CR><CR>	
    "nmap <C-@>t :scs find t <C-R>=expand("<cword>")<CR><CR>	
    "nmap <C-@>e :scs find e <C-R>=expand("<cword>")<CR><CR>	
    "nmap <C-@>f :scs find f <C-R>=expand("<cfile>")<CR><CR>	
    "nmap <C-@>i :scs find i ^<C-R>=expand("<cfile>")<CR>$<CR>	
    "nmap <C-@>d :scs find d <C-R>=expand("<cword>")<CR><CR>	


    " Hitting CTRL-space *twice* before the search type does a vertical 
    " split instead of a horizontal one (vim 6 and up only)
    "
    " (Note: you may wish to put a 'set splitright' in your .vimrc
    " if you prefer the new window on the right instead of the left

    "nmap <C-@><C-@>s :vert scs find s <C-R>=expand("<cword>")<CR><CR>
    "nmap <C-@><C-@>g :vert scs find g <C-R>=expand("<cword>")<CR><CR>
    "nmap <C-@><C-@>c :vert scs find c <C-R>=expand("<cword>")<CR><CR>
    "nmap <C-@><C-@>t :vert scs find t <C-R>=expand("<cword>")<CR><CR>
    "nmap <C-@><C-@>e :vert scs find e <C-R>=expand("<cword>")<CR><CR>
    "nmap <C-@><C-@>f :vert scs find f <C-R>=expand("<cfile>")<CR><CR>	
    "nmap <C-@><C-@>i :vert scs find i ^<C-R>=expand("<cfile>")<CR>$<CR>	
    "nmap <C-@><C-@>d :vert scs find d <C-R>=expand("<cword>")<CR><CR>


    """"""""""""" key map timeouts
    "
    " By default Vim will only wait 1 second for each keystroke in a mapping.
    " You may find that too short with the above typemaps.  If so, you should
    " either turn off mapping timeouts via 'notimeout'.
    "
    "set notimeout 
    "
    " Or, you can keep timeouts, by uncommenting the timeoutlen line below,
    " with your own personal favorite value (in milliseconds):
    "
    "set timeoutlen=4000
    "
    " Either way, since mapping timeout settings by default also set the
    " timeouts for multicharacter 'keys codes' (like <F1>), you should also
    " set ttimeout and ttimeoutlen: otherwise, you will experience strange
    " delays as vim waits for a keystroke after you hit ESC (it will be
    " waiting to see if the ESC is actually part of a key code like <F1>).
    "
    "set ttimeout 
    "
    " personally, I find a tenth of a second to work well for key code
    " timeouts. If you experience problems and have a slow terminal or network
    " connection, set it higher.  If you don't set ttimeoutlen, the value for
    " timeoutlent (default: 1000 = 1 second, which is sluggish) is used.
    "
    "set ttimeoutlen=100

"endif

"set background=dark
"set background=light
""""""""""""""""""""""""""""""""""""""""colorscheme choose """"""""""""""""""""""""""""""""""""""""

"gruvbox need background = dark
"autocmd vimenter * ++nested colorscheme gruvbox


"colorscheme monokai-bold
"colorscheme candid
"colorscheme onedark
colorscheme dogrun
