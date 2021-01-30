# vim tips and such

### yank a selection to the search register

1. `"Ayw` to yank and append a word into register A, or `"ayw` to replace register 'a'
2. `:/^R A` to put the contents of register A into the search string.

[link](https://stackoverflow.com/questions/2312844/vim-yank-into-search-register)

### System wide nvim config location
    /etc/xdg/nvim/sysinit.vim
[system wide nvim config](https://jdhao.github.io/2019/11/10/install_config_neovim_system_wide/)

### Neovim System Clipboard Support Wayland clipboard 
    clipboard+=unnamedplus
[neovim clipboard provider doc](https://neovim.io/doc/user/provider.html#clipboard)
