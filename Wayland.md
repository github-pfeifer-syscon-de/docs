# wayland

## wayfire

a nice desktop that shows nice visual effects.

```
git clone https://aur.archlinux.org/wlroots-git.git
git clone https://aur.archlinux.org/wf-config.git
git clone https://aur.archlinux.org/wayfire-git.git
git clone https://aur.archlinux.org/wf-shell-git.git
git clone https://aur.archlinux.org/wcm-git.git
```

Add .config/wayfire.ini

```
[core]
xwayland = 1
```

Alias (fix nouvea no cursor):

```
alias WF='export GDK_BACKEND=wayland; export GTK_THEME=Adwaita:dark; export WLR_NO_HARDWARE_CURSORS=1; wayfire'
```

Requires a uptodate system, on raspi it looks nice, but the older firefox crashes sometimes.
