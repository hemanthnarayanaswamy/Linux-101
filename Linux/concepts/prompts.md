# <center>Anatomy of the Prompt in Linux</center>

When you open a terminal in Linux, the line that awaits your command input is known as the shell prompt. This prompt can provide essential information about the current user, machine, directory, and other contextual data. However, the prompt isn't just a static piece of text—it's highly customizable and can be configured to provide different kinds of data according to user preferences. 

```ini
username@hostname:~$

username: is the name of the current user
hostname: is the name of the machine
~ represents the home directory of the user
$ indicates that you're a regular user 
# if your see # that means you're the root user

Example:
alex@ubuntumachine:/home/alex/projects$    --> Normal User $

root@ubuntumachine:/home/alex/projects#    --> Root User #
```
The appearance and content of the prompt are determined by the value of a shell environment variable called `PS1`

```bash
echo $PS1
\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\u@\h:\w\$
```

<table><thead><tr><th>Segment</th><th>Description</th></tr></thead><tbody><tr><td><code>\[\e]0;</code></td><td>Begins a sequence to set the title of the terminal window.</td></tr><tr><td><code>\u</code></td><td>The username of the current user.</td></tr><tr><td><code>@\h</code></td><td>The "@" symbol followed by the hostname of the machine (up to the first <code>.</code>).</td></tr><tr><td><code>: \w</code></td><td>A colon followed by the current working directory. <code>\w</code> provides the full path.</td></tr><tr><td><code>\a\]</code></td><td>Ends the sequence for setting the terminal title.</td></tr><tr><td><code>${debian_chroot:+($debian_chroot)}</code></td><td>Conditional inclusion. If <code>debian_chroot</code> is set, it will insert its value.</td></tr><tr><td><code>\u@\h:\w\$</code></td><td>Displays the username, the hostname, the current directory, and <code>$</code> or <code>#</code> depending on the user.</td></tr></tbody></table>

## [Customizing the Linux Prompt](https://cloudaffle.com/series/customizing-the-prompt/changing-information/)

The default prompt may not provide the exact information or appearance that you prefer. Customizing the prompt is a straightforward task.

<table><thead><tr><th>Escape Code</th><th>Description</th></tr></thead><tbody><tr><td><code>\a</code></td><td>An ASCII bell character (07)</td></tr><tr><td><code>\d</code></td><td>The date, in "Weekday Month Date" format</td></tr><tr><td><code>\D{format}</code></td><td>The format is passed to strftime(3) and the result is inserted into the prompt string; an empty format results in a locale-specific time representation</td></tr><tr><td><code>\e</code></td><td>An ASCII escape character (033)</td></tr><tr><td><code>\h</code></td><td>The hostname, up to the first <code>.</code></td></tr><tr><td><code>\H</code></td><td>The full hostname</td></tr><tr><td><code>\j</code></td><td>The number of jobs currently run in the background</td></tr><tr><td><code>\l</code></td><td>The basename of the shell’s terminal device name</td></tr><tr><td><code>\n</code></td><td>Newline</td></tr><tr><td><code>\r</code></td><td>Carriage return</td></tr><tr><td><code>\s</code></td><td>The name of the shell (i.e., "bash")</td></tr><tr><td><code>\t</code></td><td>The current time, in 24-hour HH:MM:SS format</td></tr><tr><td><code>\T</code></td><td>The current time, in 12-hour HH:MM:SS format</td></tr><tr><td><code>\@</code></td><td>The current time, in 12-hour am/pm format</td></tr><tr><td><code>\A</code></td><td>The current time, in 24-hour HH:MM format</td></tr><tr><td><code>\u</code></td><td>The username of the current user</td></tr><tr><td><code>\v</code></td><td>The version of Bash</td></tr><tr><td><code>\V</code></td><td>The release of Bash, version + patch level</td></tr><tr><td><code>\w</code></td><td>The current working directory</td></tr><tr><td><code>\W</code></td><td>The basename of the current working directory</td></tr><tr><td><code>\\</code></td><td>A literal backslash</td></tr><tr><td><code>\!</code></td><td>The history number of this command</td></tr><tr><td><code>\#</code></td><td>The command number of this command</td></tr><tr><td><code>\$</code></td><td>If the effective UID is 0, <code>#</code>, otherwise <code>$</code></td></tr></tbody></table>
