![logo](https://github.com/baskerville/bspwm/raw/master/resources/bspwm_logo.png)

## Description

`bspwm` is a tiling window manager based on binary space partitioning.

The windows are represented as the leaves of a binary tree.

## Configuration

`bspwm` have only two sources of informations: the X events it receives and the messages it reads on a dedicated socket.

Those messages are sent through the `bspc` program.

Therefore, the `BSPWM_SOCKET` environment variable must be defined and indicate the path to where the socket will be created.

The recommended way of defining keyboard shortcuts, is to use `xbindkeys`.

Configuration files must be placed in `$XDG_CONFIG_HOME/bspwm/`.

In that directory, `bspwmrc` (written in Lua) will be sourced and `autostart` executed.

Example configurations: [bspwmrc](https://github.com/baskerville/dotfiles/blob/master/bspwmrc), [autostart](https://github.com/baskerville/bin/blob/master/bspwm_autostart) and [xbindkeysrc](https://github.com/baskerville/dotfiles/blob/master/xbindkeysrc)

## Splitting Modes

There is only two splitting modes: *automatic* and *manual*.

The default mode is *automatic*. The *manual* mode is entered by sending a *presel* message.

        Example: insertion of a new node (number 4) into the given tree in
        *automatic* mode:

                     b                                   c
                    / \                                 / \
                   3   a              -->              4   b
                   ^  / \                              ^  / \
                     2   1                               3   a
                                                            / \
                                                           2   1
        +-------------------------+         +-------------------------+
        |            |            |         |            |            |
        |            |     2      |         |            |     3      |
        |            |            |         |            |            |
        |     3      |------------|   -->   |     4      |------------|
        |     ^      |            |         |     ^      |     |      |
        |            |     1      |         |            |  1  |  2   |
        |            |            |         |            |     |      |
        +-------------------------+         +-------------------------+

        Same departure, but the mode is *manual*, a `presel up` message
        was sent beforehand:

                     b                                   b
                    / \                                 / \
                   3   a              -->              c   a
                   ^  / \                             / \ / \
                     2   1                           4  3 2  1
                                                     ^
        +-------------------------+         +-------------------------+
        |            |            |         |            |            |
        |            |     2      |         |     4      |     2      |
        |            |            |         |     ^      |            |
        |     3      |------------|   -->   |------------|------------|
        |     ^      |            |         |            |            |
        |            |     1      |         |     3      |     1      |
        |            |            |         |            |            |
        +-------------------------+         +-------------------------+

## Messages

The syntax for the client is `bspc COMMAND ARGUMENTS...`.

The following messages are handled:

    quit
        Quit.

    get SETTING
        Return the value of the given setting.

    set SETTING VALUE
        Set the value of the given setting.

    dump
        Output the internal representation of the window tree.

    list
        Perform a dump of each desktop.

    windows
        Return the list of managed windows (i.e. their identifiers).

    close
        Close the focused window.

    presel DIR
        Switch to manual mode and select the splitting direction.

    ratio VALUE
        Set the splitting ratio of the focused window.

    cancel
        Switch to automatic mode.

    focus DIR
        Focus the neighbor node situated in the given direction. 

    shift DIR
        Focus the neighbor node situated in the given direction. 

    push DIR
        Push the fence located in the given direction.

    pull DIR
        Pull the fence located in the given direction.

    cycle CYC [--skip-floating|--skip-tiled]
        Focus the next or previous window in the list of leaves.

    rotate ROT
        Rotate the tree of the current desktop.

    send_to DESKTOP_NAME
        Send the focused window to the given desktop.

    use DESKTOP_NAME
        Select the given desktop.
    
    alternate
        Alternate between the current and the last focused desktop.

    add DESKTOP_NAME
        Make a new desktop with the given name.

    rename CURRENT_NAME NEW_NAME
        Rename the desktop named CURRENT_NAME to NEW_NAME.

    cycle_desktop CYC
        Select the next or previous desktop.
        
    layout LYT
        Set the layout of the current desktop to LYT.

    toggle_fullscreen
        Toggle the fullscreen state of the current window.

    toggle_floating
        Toggle the floating state of the current window.

    toggle_locked
        Toggle the locked state of the current window (locked windows will not respond to the 'close' command).

    rule PATTERN floating 
        Make a new rule that will floats the windows whose class name or instance name matches PATTERN. 

    reload_autostart
        Reload the autostart file.

    reload_settings
        Reload the settings file.

    reload
        Reload the autostart and the settings file.

Where

    DIR = left|right|up|down
    CYC = next|prev
    ROT = clockwise|counter_clockwise|full_cycle
    LYT = monocle|tiled

## Settings

Colors are either [X color names](http://en.wikipedia.org/wiki/X11_color_names) or '#RRGGBB'.

    active_border_color
        Color of the main border of a focused window.

    normal_border_color
        Color of the main border of an unfocused window.
    
    inner_border_color
        Color of the inner border of a window.

    outer_border_color
        Color of the outer border of a window.
    
    presel_border_color
        Color of the *presel* message feedback.

    active_locked_border_color
        Color of the main border of a focused locked window.

    normal_locked_border_color
        Color of the main border of an unfocused locked window.

    urgent_border_color
        Color of the border of an urgent window.

    inner_border_width
    main_border_width
    outer_border_width
        Width of the inner, main and outer borders.

    window_gap
        Value of the gap that separates windows.

    top_padding
    bottom_padding
    left_padding
    right_padding
        Padding space added at the sides of the screen.

    wm_name
        The value that shall be used for the _NET_WM_NAME property of the root window.

## Mouse Bindings

    M4 + Left Button
        Move the window under the pointer.

    M4 + Middle Button
        Focus the window under the pointer.

    M4 + Middle Button
        Resize the window under the pointer (by moving one of its four corners).

Where *M4* is the fourth modifier mask (generally bound to *Super*).

## Required Libraries:

libxcb, xcb-util-wm, lua.

## Installation

    make
    make install

