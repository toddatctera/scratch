# vim tips and such

### yank a selection to the search register

1. `"Ayw` to yank and append a word into register A, or `"ayw` to replace register 'a'
2. `:/^R A` to put the contents of register A into the search string.

[yank into search register](https://stackoverflow.com/questions/2312844/vim-yank-into-search-register)

### nvim config location

    # personal config
    $HOME/.config/nvim/init.vim
    # system wide config
    /etc/xdg/nvim/sysinit.vim
    # Windows personal config
    C:\Users\<logon-name>\AppData\Local\nvim\init.vim
    
[system wide nvim config](https://jdhao.github.io/2019/11/10/install_config_neovim_system_wide/)


### Neovim System Clipboard Support Wayland clipboard 
    clipboard+=unnamedplus
[neovim clipboard provider doc](https://neovim.io/doc/user/provider.html#clipboard)

### Configure Neovim to yank to Windows System Clipboard
1. Add or verify win32yank.exe exists. I installed it in my Cloud Drive which might be a bad idea but here it is:
    C:\Users\ToddButters\CloudDrive\apps\neovim\bin
4. Add neovim to Windows $PATH:
    C:\Users\ToddButters\CloudDrive\apps\neovim\bin
3. Add this to nvim config
    1. :edit $MYVIMRC
    2. set clipboard+=unnamedplus
    3. :source $MYVIMRC

[how to use the windows clipboard from wsl](https://github.com/neovim/neovim/wiki/FAQ#how-to-use-the-windows-clipboard-from-wsl)

### Example config

```
set clipboard+=unnamedplus      " yank and such to system clipboard
set number                      " show line numbers
set relativenumber              " useful for motions
set ruler                       " always show the cursor line and col position
set cursorline                  " highlight the current line
set nowrap                      " don't wrap long horizontal lines.
set splitbelow                  " split below
set splitright                  " split right
set scrolloff=8                 " scroll offset
set sidescroll=10               " see context when scrolling horizontally
```
