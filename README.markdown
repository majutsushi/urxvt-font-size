urxvt-font-size
===============

This is a perl extension for
[rxvt-unicode](http://software.schmorp.de/pkg/rxvt-unicode.html) that
allows changing the font size on the fly with keyboard shortcuts. It has
the following features:

-   Supports both xft and X11 fonts.
-   Supports X11 font aliases.
-   Supports all four font settings: `font`, `boldFont`, `italicFont`
    and `boldItalicFont` and changes them in accordance with the base
    font (the first one from `font`).
-   Can apply the font change globally for the whole server, so that new
    terminals will inherit the same size, and even save it to
    `~/.Xresources` to be able to survive a reboot.
-   Should work even with complicated font setups like the example in
    the urxvt man-page.

Installation
------------

Put the `font-size` script into `$HOME/.urxvt/ext/` and add it to the list of
enabled perl-extensions in `~/.Xresources`:

```
URxvt.perl-ext-common: ...,font-size
```

Note: if your urxvt version is older than 9.14 you will have to add the path
to the script to the `URxvt.perl-lib` resource.

Add some keybindings:

```
URxvt.keysym.C-Up:     perl:font-size:increase
URxvt.keysym.C-Down:   perl:font-size:decrease
URxvt.keysym.C-S-Up:   perl:font-size:incglobal
URxvt.keysym.C-S-Down: perl:font-size:decglobal
```

The following functions are supported:

-   increase/decrease: increase or decrease the font size of the current
    terminal.
-   incglobal/decglobal: same as above and also adjust the X server values so
    all newly started terminals will use the same fontsize.
-   incsave/decsave: same as incglobal/decglobal and also modify the
    `~/.Xresources` file so the changed font sizes will persist over a restart
    of the X server or a reboot.
