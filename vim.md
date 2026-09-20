
edit with vim:
escp
gg
dG
escp
i 
cntrl+v
escp
:wq

Common line‑deletion commands
dd – delete the current line
5dd – delete 5 lines starting from the current one
d$ – delete from cursor position to end of line
d0 – delete from cursor position to beginning of line
D – same as d$
About your guess: dg?
dg by itself isn’t a valid “delete line” operation. In Vim, delete commands follow the pattern:

d + motion

So:

dj – delete current line and the one below
dk – delete current line and the one above
dgg – delete from current line to the top of file
dG – delete from current line to the end of file
If you want to delete until a character, you’d do something like:

dfx – delete through the next occurrence of x
dtx – delete up to (but not including) the next x
If you tell me what you’re trying to delete exactly, I can show you the cleanest motion for it.

Vim has lots of navigation tricks, and in big files they become superpowers. Here’s the clean, practical list:

Basic vertical movement
j – down one line
k – up one line
5j / 5k – move 5 lines at a time
Jump directly to specific places
gg – go to top of file
G – go to bottom of file
123G – go to line 123(works with any number)
Scroll without moving your cursor
Ctrl + u – scroll half‑page up
Ctrl + d – scroll half‑page down
Ctrl + b – scroll full page up
Ctrl + f – scroll full page down
Search-based navigation (super useful in huge files)
/word + Enter – jump to next occurrence
n – next match
N – previous match
Jump by paragraphs or blocks
{ – jump to previous blank-line boundary
} – jump to next blank-line boundary
Fancy: jumps list
Vim keeps track of your navigation jumps:

Ctrl + o – jump backward in history
Ctrl + i – jump forward