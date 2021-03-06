# custom xkb symbols file 

## localectl

set the keyboard layout with localectl

```
localectl --no-convert set-x11-keymap gb pc104 mac
```

use the --no-convert option,
so we dont set a keymap in the console

check that /etc/vconsole.conf is empty

```
less /etc/vconsole.conf
```

### X configuration files

running the above localectl command will create a file at this location

/etc/X11/xorg.conf.d/00-keyboard.conf

```
# Read and parsed by systemd-localed. It's probably wise not to edit this file
# manually too freely.
Section "InputClass"
        Identifier "system-keyboard"
        MatchIsKeyboard "on"
        Option "XkbLayout" "gb"
        Option "XkbModel" "pc104"
        Option "XkbVariant" "mac"
EndSection
```

##### map ctrl to alt, alt to win, win to ctrl

create the ~/.xkb/keymap/ directory

```
mkdir -p ~/.xkb/keymap
```
export the current keymap with xkbcomp

```
xkbcomp $DISPLAY ~/.xkb/keymap/keymap.xkb
```

create the ~/.xkb/symbols directory

```
mkdir -p ~/.xkb/symbols
```

create the ~/.xkb/symbols/custom file

```
vim ~/.xkb/symbols/custom
```

add the code below to ~/.xkb/symbols/custom  
to map ctrl to alt, alt to win, win to ctrl

```
// Ctrl is mapped to Alt, Alt to Win, and Win to the Ctrl key.
partial modifier_keys
xkb_symbols "alt_win_ctrl" {
    key <LALT> { [ Super_L ] };
    key <LWIN> { [ Control_L, Control_L ] };
    key <LCTL> { [ Alt_L, Meta_L ] };
    modifier_map Control { <LCTL>, <RCTL> };
    modifier_map Mod1 { <LALT>, <RALT>, Meta_L };
    modifier_map Mod4 { <LWIN>, <RWIN> };
};
```

the symbols file is called custom   
and the xkb_symbols is called alt_win_ctrl

load the local keymap and symbols

```
xkbcomp -I$HOME/.xkb $HOME/.xkb/keymap/keymap.xkb $DISPLAY
```

add xkbcomp to your ~/xinitrc

```
vim ~/.xinitrc
```

