# RHCSA9-Notes
These are my notes for the Red Hat Certified System Administrator 9 (RHCSA 9) certificate. Feel free to use them as a study material or suggest any improvements :)

## Table of Contents

* [Using Essential Tools](#using-essential-tools)
* [Essential File Management Tools](#essential-file-management-tools)

## Using Essential Tools

### Finding the origin of commands

* `which pwd`: Shows the exact command the shell will be using. It doesn't reveal aliases.
* `type ls`: Tells us if a command is **internal**, **external**, or an **alias**.
internal commands are usually found in `/bin` whereas external commands are usually in `/usr/bin`

shell looks in the directories listed in `$PATH` to find executable commands. To execute a command in the current directory, precede it with `./`

### IO Input/Output Redirection

The destination for a redirection is a file or a device. To discard the output redirect it to `/dev/null`.

Redirector | Explanation
--- | ---
\> (1\>) | STDOUT (Overwrites)
\>\> (1\>\>) | STDOUT (Appends)
2\> | STDERR
2\>&1 | Redirects STDERR to the same destination as STDOUT
\< (0\<) | STDIN

To have the output of a program be the input of another program, we use `piping`. e.g. `ps aux | less`. In piping, all the processes are launched asynchronously.

### History

Bash is configured by default to keep the last 1000 commands a user used. `.bash_history` contains these information and it is updated when a shell session is closed.

The `history` command shows a list of all commands in the Bash history.

Option | Description
--- | ---
`-d number` | deletes a specific entry
`-c` | clears the current history
`-w` | writes current history to `.bash_history`

* **!number**: execute a command with a specific number
* **!sometext**: execute the last command containing a specific text

We can type `history -w` right after `history -c` to erase the contents of `.bash_history`

### The Shell Environment

Variable names start with `$` sign. e.g, `$PATH`. To view the variables defined in the current shell environment, we can use the `env` commands.

The four configuration files:
* **/etc/profile**: Processed by all users upon login
* **/etc/bashrc**: Processes when subshells are started
* **~/.bash_profile**: User-specific login shells are defined
* **~/.bashrc**: User-specific files for subshells

Messages in `/etc/motd` display after user has successfully logged in to a shell. Convenient way to inform users about an issue or a security policy. Messages in `/etc/issue` are displayed before log in. These are good to specify login instructions.

### Finding Help

Nearly all commands support the `--help` option and will display a usage summary when using this option. Another option is the `man` command. In the `man` page, the most useful information which are the examples are located at the bottom of the page.
To search in the **mandb** database, we can use `man -k` or `apropos` which are equivalent commands. We can then filter out the result using the `grep` command, more on this later.
To update the **mandb** database, use `mandb`. `apropos` code sections:
* **1**: Executable programs or shell commands
* **5**: File formats and conventions
* **8**: System administration commands
The two other tools to get even more information about the system are `info` and `pinfo`. And finally, we can find the documentation of large projects in `/usr/share/doc`.

## Essential File Management Tools

### The File System Hierarchy (FSH)

A **mount** is a connection between a device and a directory. the **root directory (/)** is the starting point.Below is an overview of the FHS:

Directory | Use
--- | ---
**/** | The root directory (starting point)
**/boot** | All the stuff required to boot the Linux
**/dev** | Files for accessing physical devices
**/etc** | All the configurations. e.g. `profile` or `bashrc`
**/home** | For local user home directory. Most users spend most of their time here
**/media,/mnt** | For mounting external devices
**/opt** | Optional packages
**/proc** | Proc file system, gives access to kernel information
**/root** | The home directory for the root user
**/run** | User-specific information since last boot
**/srv** | Used for data services like NFS, FTP, HTTP
**/sys** | Interface to different devices managed by the Linux kernel
**/tmp** | Temporary files that can be deleted without warning
**/usr** | Subdirectories for programs, libraries, and their documentations. e.g. `/usr/bin`
**/var** | Files that may change in size dynamically. e.g. `log files`
