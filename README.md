# RHCSA9-Notes
These are my notes for the Red Hat Certified System Administrator 9 (RHCSA 9) certificate. Feel free ro use them as a study material or suggest any improvements :)

## Using Essential Tools

### Finding the origin of commands

* `which pwd`: Shows the exact command the shell will be using. It doesn't reveal aliases.
* `type ls`: Tells us if a command is internal, external, or an alias.

shell looks in the directories listed in `$PATH` to find executable commands. To execute a command in the current directory, precede it with `./`
