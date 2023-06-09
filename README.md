# RHCSA9-Notes
These are my notes for the Red Hat Certified System Administrator 9 (RHCSA 9) certificate. Feel free to use them as a study material or suggest any improvements :)

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
