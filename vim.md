(under normal mode and visual modes)
(modes)
i insert before character
o insert in a new line below
a insert after character
v visual mode select
V visual line mode select line
R replace mode

(edit)
ctrl-v visual block mode block
d delete(cut) a line
x delete a letter
y copy
p paste
u undo
= (after selection) auto indent


(move/location)
hjkl left down up right
gi move to the last edit place and insert
w move to the head of the next word 
b move to the head of the last word
0 move to the head of the line
$ move to the tail of the line
gg move to the head of the file
G move to the tail of the file 
ctrl-u ctrl-f pageUp/pageDown
zz set the cursor line to the middle (for vision)
zc fold the code
zo unfold

(line command mode)
:set number display line number
:vs split left-right
:sp split top-bottom
:% s/*from*/*to*/g replace globally
:help *cmd* search for help info
:syntax on 
:!cmd invoke Linux shell cmd

(search mode)
/*pat* search for the pattern
n search next
N search last

(mutifile edit)
:vs filename
:sp filename
ctrl-w+hjkl move around

(under insert mode)
ctrl-h delete last letter
ctrl-w delete last word
ctrl_u delete a line
ctrl_a to the line head
ctrl_e to the line end
ctrl_[ exit insert mode

(map config)
map/imap/vmap -> noremap/inoremapl/vnoremap
let mapleader=','
inoremap from to
noremap from to
!cmd from to 

(NERDTree)
o open a dir/file
x fold dir
u go up dir
i/s open veticle/horizontal
go/gi/gs preview, similar to o/i/s
cd change current work directory
r refresh
q exit the NERDTree

(common config)
,w save under insert mode (but ',' will be a bit slower)
jj to normal mode
<c-w>h -> <c-h>(and so on) faster multiwindow switch

(vim-plug)
warning:the color scheme plugin must be load before the command "colorscheme .."
Plugin Install:
1.place 'Plug ...(github suffix)' to .vimrc
2.source .vimrc or restart vim
3.:PlugInstall
Plugin Uninstall:
1.remove the 'Plug ..' line
2.source .vimrc or restart vim
3.:PlugClean

(tmux)
1.fix the color:
edit ~/.tmux.conf and put: 
set -g default-terminal "xterm-256color"
into the file
2.basic op
prefix+% new verticle split
prefix+" new horizontal split
prefix+direct move around
prefix+direct(not release) resize
prefix+c create new window
prefix+n switch window
3.config
set -g default-terminal "xterm-256color"

## emmet
- let g:user_emmet_leader_key='<tab>'
and enter <tab>,

## UltiSnips
- 获得帮助：
  :h UltiSnips
- 关键配置
  `let g:UltiSnipsSnippetDirectories=["UltiSnips"]`
  这个选项是设置snippets文件放置的目录，对应~/.vim/UltiSnips
- snippets注意
  - 主文件名对应拓展名（如cpp.snippets），不可无中生有（如tmp.snippets），不要漏掉s
  - all对应所有



(miscellaneous)
- v + s Quick replace
- macro: q+any name to start recording, q to quit; (Normal mode)@+name to apply the macro.
