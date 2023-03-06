{} - move paragraph 
:h j - help for j letter :q to close
gg - start of a file
NUMBgg - go to line
G end of a file
. - repeat last command
# Delete 
anything deleted is in our clipboard

dd - delete line
NUMdd - delete number of lines
2dd - remove 2 lines
x - remove chars
NUMBx - delete number of chars
2x - remove 2 chars
d$ - delete to the end of the line

## Visual mode
down arrow highlight line
d - to delete

# Copy
yy - copy line
numb

# Past
p

# replace
r - releact
# Undo 
U
## Redo
CTRL R


# edit/open
:e - open other file

# search

/
n - next result
N - back result
/word/i - case insensitive 
[] - search for letters
/word$ - end of the line word

## search replace/delete
:s/word/otherword/g - replace word with otherword /g means global more 
:%s/word/otherword/g - replace word with otherword /g means global more than once per line
g/word/d - delete the word word globally
v/word/d - delete all lines doesn't have word word in it 

### shift v for visual mode
move with arrows to replace only words inside selected

# tabs
:tabnew - create new tab
:tabclose - close tab
:tabnext - go to next tab
:tabpreviouss - go to previous tab
gt - go forward
gT - go bbackwards

## split pane
CTRL +W

# commands
:! - start command
:%! 
# Register
clipboard slots
:registers 
"1p retrieve from register 

# numbers
set numbers on
:set nu

# plugins
.vimrc 
[mattaggart_vimrc](https://gist.github.com/mttaggart/b788e618eddca83a9c033572bfdf3c76)
```
set nu 
set wrap linebreak nolist
set clipboard=unnamedplus
set tabstop=4 softtabstop=0 expandtab shiftwidth=4 smarttab
set encoding=utf-8
call plug#begin()
Plug 'scrooloose/nerdtree'

call plug#end()
```
:Pluginstall= - install plugins
:Plugupdate: update plugins
[vim_plug_manager](https://github.com/junegunn/vim-plug](https://github.com/junegunn/vim-plug)
[vim plugins]( [https://vimawesome.com](https://vimawesome.com/))
