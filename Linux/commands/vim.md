# [vim](https://opensource.com/article/19/3/getting-started-vim)

## Modes in VIM
1. `NORMAL`: 
* In normal modes key presses doesn't work, this mode is normally for `move, delete, copy`, when you exit you return to this mode. 

2. `INSERT`:
* You can start to type text in this mode, just like a regular text editor. 
* Press `i` to enter into INSERT mode and `esc` to exit from the mode. 

3. `VISUAL`:
* Visual mode is used to make selections of text, similar to how clicking and dragging with a mouse behaves. 
* Selecting text allows commands to apply only to the selection, such as `copying, deleting, replacing`. 
* Press `v` to enter to the `visual` mode. 
* Press `ctrl + v` to enter `block-visual`, select any rectangle region. 
* Press `shift + v` to enter `linewise-visual`, always select full lines. 

4. `COMMAND`:
* Command mode has a wide variety of commands and can do things that normal mode can't do as easily. 
* To enter command mode type `:` from normal mode and type your command, which should appear at the bottom of the window. 
* Eg: `:%s/foo/bar/g` to replace all **foo** with **bar**. Here `%` means across all lines. 

---
## Basic Movements
* `0`: Move to the start of line 
* `$`: Move to the End of line 
* `w`: Move to next word in line 
* `b`: Move to previous word in line
* `gg`: Move to first line
* `G`: Move to Last line
* `:n`: where n is the line number, Jump to that line number. 


* `H, M, L`: Move cursor to Top, middle, Bottom of screen. 
* `zz`: Move cursor to centre of screen. 

---
## Delete and UNDO
* `dd`: Delete a line 
* `u`: UNDO Operation 
* `ctrl + r`: REDO Operation 

---
## Copy and Paste
* `yy`: copy line 
* `p`: paste below
* `P`: Paste above

---
## Visual Selection 
* `ctrl + v`: Gets you into the visual mode, and you need to use the **arrow keys to select the multiple lines**, **dont use mouse cursor, it won't work**. 
* After you see the selection, then you can perform operations like, `y -> Copy`, `d -> Delete`. 
* For `Indentation`, use `<`, `>`, this will only perform the indentation once, but to repeat the indentation use `.`.
* After any indent or unindent command, you can repeat the operation by pressing the `. (period) key`.

---
## VIM SEARCH

* Ensure you are in Normal mode by pressing the `ESC` key.
* Initiate a forward search:
    - Type `/` `(forward slash)`.
    - Enter the word or pattern you want to search for. Press Enter.
    - Vim will highlight the first occurrence of the pattern and move your cursor to it.
* Press `n` to move to the next occurrence of the serached pattern, Press `N` to move to the previous occurrence of the searched pattern. 
