# The `tmux` commands

`tmux` lets you run many terminal sessions inside a single window, but its real superpower is persistence: a tmux session keeps running on the machine even after you disconnect.

- You can start a long job on a remote server, detach, close your laptop, and reconnect hours later to find everything exactly as you left it — output and all.
- For anyone working over `SSH`, this alone makes it indispensable, because a dropped connection no longer kills your work.

![tmux](../../../resources/assets/tmux.png)

```bash
sudo apt update
sudo apt install tmux
```
### Fundmental Concepts of TMUX

1. **Session**: A session is an independent workspace that can contain multiple windows. You can think of a session as a project or a task. For example, you might have one session for web development and another for system administration.
2. **Window**: A window is a single visible screen within a session. Each session can have multiple windows, and you can switch between them easily. Windows are similar to tabs in a web browser.
3. **Pane**: A pane is a subdivision of a window. You can split a window horizontally or vertically to create multiple panes, allowing you to view and work on multiple terminal sessions simultaneously.

Tmux is a terminal `multiplexer`; it allows you to create several "pseudo terminals" from a single terminal.
**Tmux also decouples your programs from the main terminal, protecting them from accidentally disconnecting. You can detach tmux from the current terminal, and all your programs will continue to run safely in the background.**

---
## Tmux Usage

### Basic Commands
```bash
tmux # To start a new tmux session

tmux new -s <my sesssion name> # Start a session with a specific name

tmux ls     # to list all active sessions.

tmux attach -t my_session_name  #to reattach a session.

tmux kill-session -t my_session_name  # to end a session.
```
[TMUX CHEATSHEET](https://tmuxcheatsheet.com/)

### Basic Key Bindings
tmux uses a prefix key (by default `Ctrl + b`) to execute commands. After pressing the prefix key, you can use various key combinations to perform different actions.

> prefix keys refer to key sequences whose bindings are defined in a keymap.

<table><thead><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Key</span></p></th><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Description</span></p></th></tr></thead><tbody><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Ctrl+B D</span></p></th><td style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>It will detach from the current session.</span></p></td></tr><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Ctrl+B %</span></p></th><td style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>It will split the window into two panes horizontally.</span></p></td></tr><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Ctrl+B " </span></p></th><td style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>It will split the window into two panes vertically.</span></p></td></tr><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Ctrl+B Arrow Key (Left, Right, Up, Down)</span></p></th><td style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>It helps in moving between panes.</span></p></td></tr><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Ctrl+B N or P </span></p></th><td style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>It helps in switching the next or previous window.</span></p></td></tr><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Ctrl+B C</span></p></th><td style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>It will create a new window.</span></p></td></tr><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Ctrl+B X </span></p></th><td style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>It will close the pane</span></p></td></tr><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Ctrl+B 0 (1,2...)</span></p></th><td style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>It will move to the particular window by number.</span></p></td></tr><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Ctrl+B :</span></p></th><td style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Enter the command line to type commands.</span></p></td></tr><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Ctrl+B ? </span></p></th><td style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>It will display all key bindings.</span></p></td></tr><tr><th style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>Ctrl+B W</span></p></th><td style="width: 350px;"><p dir="ltr" style="text-align: center;"><span>It opens a panel to navigate across windows in multiple sessions.</span><br></p></td></tr></tbody></table>

### Configuration File

Create a `~/.tmux.conf` file in your home directory to customize tmux settings.

```bash
# Set the prefix key to Ctrl+a (easier to press than the default Ctrl+b) 
unbind C-b set -g prefix C-a 
bind C-a send-prefix  

# Enable mouse support (you can click to switch panels/windows) 
set -g mouse on  

# Set status bar color 
set  -g status-bg black 
set -g status-fg white  

# Set panel border color 
set  -g pane-border-style fg=green  
set  -g pane-active-border-style fg=red   

# Set window numbering starting from 1 (default is 0): 
set -g base-index 1 
setw -g pane-base-index 1
```
