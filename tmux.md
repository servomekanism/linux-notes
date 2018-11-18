[//]: # (tags: tmux)

### start new with session name:
`tmux new -s myname`

### attach
`tmux a  #  (or at, or attach)`

###attach to named:
`tmux a -t myname`

### list sessions:
`tmux ls`

### kill session:
`tmux kill-session -t myname`

## Panes
### Panes (splits)
`%`  vertical split

`"`  horizontal split

`o`  swap panes

`q`  show pane numbers

`x`  kill pane

`+`  break pane into window (e.g. to select text by mouse to copy)

`-`  restore pane from window

`⍽`  space - toggle between layouts

`<prefix> q` (Show pane numbers, when the numbers show up type the key to goto that pane)

`<prefix> {` (Move the current pane left)

`<prefix> }` (Move the current pane right)

`<prefix> z` toggle pane zoo

## Misc
`d`  detach
`t`  big clock
`?`  list shortcuts
`:`  prompt

## Managing split panes
### Creating a new pane by splitting an existing one:
`C-b "`          split vertically (top/bottom)
`C-b %`          split horizontally (left/right)

### Switching between panes:
`C-b left`       go to the next pane on the left

`C-b right`      (or one of these other directions)

`C-b up`

`C-b down`

`C-b o`          go to the next pane (cycle through all of them)

`C-b ;`          go to the ‘last’ (previously used) pane

### Moving panes around:
`C-b {`          move the current pane to the previous position

`C-b }`          move the current pane to the next position

`C-b C-o`        rotate window ‘up’ (i.e. move all panes)

`C-b M-o`       rotate window ‘down’

`C-b !`          move the current pane into a new separate
               window (‘break pane’)
`C-b :` move-pane -t :3.2
split window 3's pane 2 and move the current pane there

### Resizing panes:
`C-b M-up, C-b M-down, C-b M-left, C-b M-right`
               resize by 5 rows/columns
               
`C-b C-up, C-b C-down, C-b C-left, C-b C-right`
               resize by 1 row/column
               
## Applying predefined layouts:
`C-b M-1`        switch to even-horizontal layout

`C-b M-2`        switch to even-vertical layout

`C-b M-3`        switch to main-horizontal layout

`C-b M-4`        switch to main-vertical layout

`C-b M-5`        switch to tiled layout

`C-b space`      switch to the next layout
## Other:
`C-b x`          kill the current pane

`C-b q`          display pane numbers for a short while
