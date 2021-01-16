# Fixes 

### “E437: terminal capability “cm” required”

I would get this error on Fedora using Alacritty when ssh'd to our CentOS 7 server.
Quick fix was to set term. Permanent fix would be to put this in `~/.bashrc`

    export TERM=xterm-256color

https://blog.slowb.ro/fix-terminal-capability-cm-required/

### open terminal failed: missing or unsuitable terminal: alacritty

Same as above but when opening tmux. If I put these in `~/.tmux.conf` it would fix it.
But after putting the above in `.bashrc` it no longer seems necessary.

    set -g default-terminal screen-256color
    set -g terminal-overrides ",alacritty:RGB"

https://github.com/alacritty/alacritty/issues/2487
