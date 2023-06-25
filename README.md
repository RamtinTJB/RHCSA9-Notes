# RHCSA9-Notes
These are my notes for the Red Hat Certified System Administrator 9 (RHCSA 9) certificate. Feel free to use them as a study material or suggest any improvements :)

## Table of Contents

* [Using Essential Tools](#using-essential-tools)
* [Essential File Management Tools](#essential-file-management-tools)
* [Working with Text Files](#working-with-text-files)

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

Reasons to work with multiple mounts:
* High activity in one area won't fill up the entire system
* Possible to set different security options for different devices
* Easy to make additional storage available
`/boot` and `/boot/EFI` are usually on separate devices because the Kernel cannot access the root directory during boot. Since `/var` can vary in size it also usually has a dedicated mount. The other two directories that are commonly on separate devices are `/home` and `/usr`

### Getting Information about Current Mounts

* **`mount`**: This command gives an overview of all the mounted devices. It reads this information from `/proc/mounts`
* **`df -Th`**: It shows available disk space on mounted devices. `-h`: human readable, `-T`: Show filesystem
* **findmnt**: Shows mounts and their relationships

### Managing and Working with Directories

Below is the summary of the commands and options we need to know in this section:
* `cd`: moving between directories
* `mkdir`: creating a directory
* `pwd`: displaying the current directory's absolute path
* `ls`: listing files and directories. A useful option is `-lrt` which lists the most recently modified files last.
* `touch`: creating a new file
* `cp`: copying files.
    * To keep the current permissions of the files, use `-a`
    * To copy all files, regular and hidden: `cp -a /somedir/. .`
    * To copy only hidden files: `cp /somedir/.* /tmp`
* `mv`: moving files and directories
* `rm`: removing files and directories. Use the `-rf` option to delete a folder with all its contents

### Using Hard and Soft Links

Linux stores all the administrative information of the files in **inodes**. **inodes** contain the following the information:
* The data block where the file contents are stores
* The creation, access, and modification date
* Permissions
* File owners

The name of the file is referred to as a **hard link**, so every file has at least one **hard link**.
All **hard links** must exist on the same device, they cannot point to directories, and when the last hard link is removed, file data is also removed.

**Symbolic links** on the other hand point to the name of the file which makes them more flexible. They can point to files on other devices and directories.

To create links we use the following command: `ln [-s] file/folder link-name`. `-s` makes it a symbolic link. e.g. `ln -s /etc/hosts .` creates a symbolic link to the file `/etc/hosts` in the current directory.

We can view the links using `ls -l`. if the file is a symbolic link, the first character of the line is going to be `l`. If we file is a hard link, the output will the hard link counter.

### Working with Archives and Compressed Files

the Tape ARchiver (**tar**) is used to manage archive files. we usually use `-vf` options with all the other options. `-v` makes the output verbose and `-f` specifies the tar file we intend to work with. options:

* `-c`: creating an archive. `tar -cvf /root/homes.tar /home`
* `-u`: update an existing archive. `tar -uvf /root/homes.tar /home`
* `-t`: list the contents of an existing archive
* `-x`: extract an archive. Can be used with `-C` to specify the target directory of extraction
* `-z (gzip), -J (xz), -j (bzip2)`: to compress while creating an archive. No need to use these for extraction, **tar** will automatically detect the compression method during extraction.

## Working with Text Files

### Common Text File Tools

* **less**: opens the text in a pager, allows for easy reading of long texts. The controls inside `less` are similar to `vim`.
    * `f` and `b`: Go one page forward or backward
    * `/` and `?`: Used for forward and backward searching. Repeat the search by pressing `n`.
    * `q`: To quit `less`
* **cat**: dumps the contents of the file on the screen.  
* **head** and **tail**: these two  commands will show the first or last n lines of a file. 
    * `tail -n 5 /etc/passwd` = `tail -5 /etc/passwd` shows the last 5 lines of the file `/etc/passwd`
    * `head -11 /etc/passwd | tail -1` will show line number 11 of the `/etc/passwd` file
    * `tail -f` will watch the file in real time
* **cut**: used for filtering specific columns.
    * `cut -d : -f 1 /etc/passwd` will list all the users because the fields in the file `/etc/passwd` are separated by `:` and the username is the first field (`-f 1`)
