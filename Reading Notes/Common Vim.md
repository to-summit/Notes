## Common Options
vim \[OPTIONS\] \[FILE ...\]
<ol>
    <li>-R          # read-only(also: view file)</li>
    <li>-O          # open files in vertical splits </li>
    <li>-o          # open files in horizontal splits </li>
    <li>-p          # open files in tabs </li>
    <li>-d          # diff mode(also: vimdiff) </li>
    <li>-u FILE     # use FILE as vimrc(use NONE for no vimrc) </li>
    <li>--noplugin  # skip plugin loading </li>
    <li>-c CMD      # run CMD after loading </li>
    <li>-S SESSIONO # source SESSION file </li>
    <li>+N FILE     # open at line N </li>
    <li>+/PAT FILE  # open at first match of PAR </li>
    <li>-b          # binary edit mode </li>
    <li>--clean     # ignores user config </li>
</ol>

## Common command
### Search and replace in normal mode
:%s/old/new/g          # all occurrences in file
:%s/old/new/gc         # confirm each replacement
:5,20s/old/new/g       # only on lines 5-20
:%s/\<word\>/new/g     # whole word match

### Search history and navigation
/pattern               # search forward
?pattern               # search backward
n / N                  # next / previous match

### Buffers and windows
:e file.txt            # open another file in current window
:bnext / :bprev        # cycle buffers
:bd                    # close buffer
:vsp file.txt          # vertical split with file
:sp                    # horizontal split same buffer
Ctrl-W h/j/k/l         # navigate splits

### Tabs
:tabnew file.txt
:tabnext / :tabprev
gt / gT                # next / prev tab

### Macros (record, replay)
qa                     # start recording into register a
... commands ...
q                      # stop recording
@a                     # play register a
@@                     # repeat last macro

### Registers (named clipboards)
"ay                    # yank into register a
"ap                    # paste from register a
"+y                    # yank into system clipboard (needs +clipboard)
"+p                    # paste from system clipboard

## Tricks
### Quick file-tree without plugins
:e .                   # netrw built-in file browser
:Vex                   # vertical explore

### Open last quickfix list (after :grep, :make)
:copen
:cnext / :cprev

### Format JSON in current buffer (*filter* in vim)
:%!jq .

### Format SQL with sqlformat
:%!sqlformat -

### Open vim with multiple files in one tab via vim -p
vim -p src/*.c

### delete every line (NOT) matching a regex; 
:g/pattern/d :v/pattern/d 
