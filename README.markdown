urxvt-font-size
===============

This is a perl extension for
[rxvt-unicode](http://software.schmorp.de/pkg/rxvt-unicode.html) that allows
changing the font size on the fly with keyboard shortcuts. It has the
following features:

-   Supports both xft and X11 fonts; X11 fonts work in both full form and as
    aliases.
-   Supports all four font settings: `font`, `boldFont`, `italicFont` and
    `boldItalicFont` and changes them in accordance with the base font (the
    first one from `font`).
-   Can apply the font change globally for the whole server, so that new
    terminals will inherit the same size, and even save it to `~/.Xresources`
    to be able to survive a reboot.
-   Should work even with complicated font setups like the example in the
    urxvt man-page.

Installation
------------

Put the `font-size` script into `$HOME/.urxvt/ext/` and add it to the list of
enabled perl-extensions in `~/.Xresources`:

```
URxvt.perl-ext-common: ...,font-size
```

Note: if your urxvt version is older than 9.14 you will have to add the path
to the script to the `URxvt.perl-lib` resource.

The extension automatically binds Ctrl++ to the 'increase' function,
Ctrl+- to 'decrease', and Ctrl+0 to 'reset'. To use the other available functions
or change the keys, add some keybindings of your own:


```
URxvt.keysym.C-Up:     font-size:increase
URxvt.keysym.C-Down:   font-size:decrease
URxvt.keysym.C-S-Up:   font-size:incglobal
URxvt.keysym.C-S-Down: font-size:decglobal
URxvt.keysym.C-equal:  font-size:reset
URxvt.keysym.C-slash:  font-size:show
```
Then load your new `~/.Xresources` to see the changes in the next terminal you open:

```
$ xrdb -merge ~/.Xresources
```

Note that for urxvt versions older than 9.21 the resources have to look like this:

```
URxvt.keysym.C-Up:     perl:font-size:increase
URxvt.keysym.C-Down:   perl:font-size:decrease
URxvt.keysym.C-S-Up:   perl:font-size:incglobal
URxvt.keysym.C-S-Down: perl:font-size:decglobal
URxvt.keysym.C-equal:  perl:font-size:reset
URxvt.keysym.C-slash:  perl:font-size:show
```

The following functions are supported:

-   `increase`/`decrease`: Increase or decrease the font size of the current
    terminal.
-   `incglobal`/`decglobal`: Same as above and also adjust the X server values
    so all newly started terminals will use the same fontsize.
-   `incsave`/`decsave`: Same as incglobal/decglobal and also modify the
    `~/.Xresources` file so the changed font sizes will persist over a restart
    of the X server or a reboot.
-   `reset`: Reset the font size to the value of the resource when starting
    the terminal.
-   `show`: Show the current value of the `font` resource in a popup.

You can also change the step size that the script will use to increase the
font size:

```
URxvt.font-size.step: 4
```

The default step size is 1. This means that with this setting a size change
sequence would be for example 8->12->16->20 instead of 8->9->10->11->12 etc.
Please note that many X11 fonts are only available in specific sizes, though,
and odd sizes are often not available, resulting in an effective step size of
2 instead of 1 in that case.

Troubleshooting
---------------

At its core urxvt-font-size is just a fancy wrapper around the `ESC ] 710;Pt
ST` (and 711/712/713 for bold, italic, and bold-italic) escape sequences. So
if you are having trouble with this script please try the escape sequence
directly like this (outside of a terminal multiplexer like screen or tmux):

```
$ echo -e "\033]710;fixed\033\\"
```

You can exchange `fixed` for any (single) font that you want to try, like for
example `-xos4-terminus-medium-r-normal--12-120-72-72-c-60-iso10646-1`:

```
$ echo -e "\033]710;-xos4-terminus-medium-r-normal--12-120-72-72-c-60-iso10646-1\033\\"
```

If you can reproduce the problem this way then the problem does not lie with
the urxvt-font-size script, but either directly with urxvt or some other
component that may interfere (like for example a window manager). Please
report the bug to those projects in that case.
