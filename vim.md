# vim tips and such

### yank a selection to the search register

1. `"Ayw` to yank and append a word into register A, or `"ayw` to replace register 'a'
2. `:/^R A` to put the contents of register A into the search string.

[yank into search register](https://stackoverflow.com/questions/2312844/vim-yank-into-search-register)

### System wide nvim config location
    /etc/xdg/nvim/sysinit.vim
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
