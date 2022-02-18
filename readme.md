# Frontend Master Complete Intro to Linux and the Command-line

## Anatomy of a Command Line Interface

What we see when we open a terminal is a command line prompt. Basically command
line prompt is a REPL (Read, Evaluate, Print, Loop) for shell. REPL is an
interactive way of programming where we are writing one line of code at a time.

Using command line prompt is similar like we're using REPL for python or node,
we can also do for loop in a command line prompt like this:
```sh
editors=(vim nvim) && for x in ${editors}; do echo $x; done
```
that for loop command is basically install all the package in file
`package-list.txt`.

The difference is that, we need to learn the shell script syntax. There's
POSIX compliant shell script and non-POSIX compliant shell script, so be
careful about the difference of those two syntax. Also there's a slight
different between POSIX compliant shell syntax such as BASH and ZSH, please
keep that in mind too.

### Emulators

The shell we're usually using is running inside an emulator, that's why we
usually call it Terminal Emulator. Because the real terminal (called TTY) is
running the GUI environment that the end-user mostly use.

## Bash History Search

We can search the command history in bash by using up/down arrow key, and
`ctrl-r` key. With `ctrl-r` key, we can search the last command we use based on
the pattern, while using up/down key we just going through the last command we
use until we found the command we want (we can't enter the command pattern with
up/down key method).

## Signals

A signal is a notification that we send to a program. Let's say we have program
running and we want to end the program early, we can send a signal to the
program to end early.

Signal doesn't force the program to do anything, signal just tell the program
of the user intent and it's up to the program to do something about it.

**To check all the signals, we can use `kill -l` command**.

### SIGINT

`SIGINT` is stands for signal interrupt, which basically means interrupt
the running program and a lot of times means stop running.

Usually the keyboard shortcut for `SIGINT` in shell is `ctrl-c`.

### SIGQUIT

`SIGINT` is like "stop doing what we're doing", meanwhile `SIGQUIT` is quit
the entire program.

Usually the keyboard shortcut for `SIGQUIT` in shell is `ctrl-d`.

> Basically try `ctrl-c` first, but if that doesn't work, try `ctrl-d`.

### SIGKILL

`SIGKILL` is basically a program assassination, the program get shut down
immediately, didn't get to clean up and didn't get any options.

> Basically we can't use `ctrl-d` on the program, like the GUI program, we
> might want to use `SIGKILL`.

## Wildcards and Replacements

Let's say we want to create `file1.txt`, `file2.txt`, `file3.txt`, and
`file4.txt`, instead of using:
```sh
touch file1.txt file2.txt file3.txt file4.txt
```
we can do something like this:
```sh
# don't put the space between 1,2,3,4.
touch file{1,2,3,4}.txt
```

What those curly brackets did is a replacement. Basically, for every item in
the curly brackets, expand that.

The nice thing about this method is that it's not specific feature of `touch`,
but it's from the shell. So we can use that with other command too.

> This is not limited to number.

Now let's say we want to look for `file1.txt`, `file2.txt`, `file3.txt`, and
`file4.txt`, but not `file.txt`. If we use `*` like this:
```sh
ls file*.txt
```
that will also print out `file.txt`, not only `file1.txt`, `file2.txt`,
`file3.txt`, and `file4.txt`. So, if we want to exclude `file.txt`, we can do
something like this:
```sh
ls file?.txt
```

Another thing we can do to create `file1.txt`, `file2.txt`, `file3.txt`, and
`file4.txt` is something like this:
```sh
touch file{1..4}.txt
```
what that command did was expand 1 through 4 so that the input of `touch`
command would be something like this `touch file1.txt file2.txt file3.txt
file4.txt`

That method is not only for number, we can also use alphabet like this:
```sh
touch file{a..d}.txt
```

We can also skip at certain occurrence like this (only in bash):
```sh
echo {a..z..2}
```

We can also create permutation with something like this:
```sh
# the output is a1, a2, a3, a4, a5, b1, ...
echo {a..z}{1..5}
```

## Environment Variables

If we want to see all the environment variable, we can use this command:
```sh
printenv
```

Let's say we want to add `EDITOR` environment variable, we do that with
something like this:
```sh
EDITOR=vim
```

The downside of that approach is that it's only in one session, so if we exit
from that shell, we lost the environment variable `EDITOR`. If we want to make
it permanent, we need to put it in our shell rc or something similar like this:
```sh
export EDITOR=vim
```

## Process

Let's say we have a process that's gonna be running for a long time and we
don't want to sit here and wait until those process to finish, so what we can
do is using `ctrl-z` which basically suspend the process and bring us back into
the interactive shell. We can use `jobs` to see the suspended process with the
output something like this:
```sh
# on bash
[1]+  Stopped                 sleep 1000

# on zsh
[1]  + suspended  sleep 1000
```
and then we can resume the process using this command:
```sh
# `1` is from `[1] + suspended ...` above
# on bash
bg 1

# on zsh
bg %1
```
which means the suspended process 1 will resume the process in the background.

And then we can do something like this if we want to see the output of the
process:
```sh
# on bash
fg 1

# on zsh
fg %1
```

## Exit Codes

Every time our program finish, it return exit code which basically means how
the previous process finished.

We can check the last command exit code with this command:
```sh
echo $?
```

**Anything that returns other than zero (0), did not finish successfully**.

> It's up to the program to choose the exit code if it's not finish
> successfully and it's not enforced by the shell. But, there are a few common
> one like this:<br>
> Exit Code | Explanation
> ---       | ---
> 0         | means it was successful. Anything other than 0 means it failed.
> 1         | a good general catch-all "there was an error".
> 2         | a bash internal error, meaning we or the program tried to use bash in an incorrect way.
> 126       | Either we don't have permission or the file isn't executable.
> 127       | Command not found.
> 128       | The exit command itself had a problem, usually that we provided a
> non-integer exit code to it.
> 130       | We ended the program with CTRL+C.
> 137       | We ended the program with SIGKILL.
> 255       | Out-of-bounds, we tried to exit with a code larger than 255.

> There's `true` command that always return exit code 0 and `false` command that
> always return exit code 1.

## SSH

SSH stands for Secure Shell, and allows us to connect from one computer to
another computer and allows us to execute command on that computer.

If we want to check the IP address for ssh usage, we can use this command:
```sh
ip addr show
```
and then look for the `inet` part, that's the public IP address.

## SFTP

Let's say we have files on computer 1 and we need to get them to computer 2, we
can do that kind of connection via FTP (File Transfer Protocol).

> A variation of FTP called SFTP (Secure File Transfer Protocol).

If we already set ssh before, we can use this command:
```sh
# similar to ssh arguments
sftp <username>:<public-ip-address>
```

> When we setup ssh, we're inherently setting up sftp as well because they use
> the same protocol, ports, basically everything kind of works the same.

To check the current working directory on the remote machine (which is computer
2 in this case), we can use this command:
```sh
# similar to usual pwd command
pwd
```
but, if we want to check the current working directory on the local machine (
which is computer 1 in this case), we need to use this command:
```sh
# `l` here basically means local
lpwd
```
likewise with the `ls` command in local machine like this:
```sh
lls
```

Now, let's say we want to put `file.txt` from our local computer to remote
computer, we can do something like this:
```sh
put file.txt
```

We can run command in our local machine shell from sftp using the `!`
command like this:
```sh
!echo 'something big' >> file.txt
```

> If we forget about something, we can use `help` command to check all the
> available command.

**Keep in mind when we're using sftp, we're in two directory. Local machine
directory and remote machine directory**.

We can also get the file from remote machine to local machine like this:
```sh
# `file.txt` is a file on the remote machine.
# `get-the-file.txt` is the supposed name on the local machine.
# basically we rename `file.txt` to `get-the-file.txt` in local machine.
get file.txt get-the-file.txt
```

One of the command to exit sftp is:
```sh
bye
```

## Using cURL For API Development

We can use curl to check the response of API endpoint and see if the response
is what we're expecting.

We can do GET request using something like this:
```sh
curl http://<our-public-ip-address>:<port>
```

We can do POST request using something like this:
```sh
# `POST` is an HTTP verbs.
curl -X POST http://<our-public-ip-address>:<port>
```
and if we want to send POST request body, we can do something like this:
```sh
curl -d "this the post request body" http://<our-public-ip-address>:<port>
```

We can do PUT request using something like this:
```sh
curl -X PUT -d "this the put request body" http://<our-public-ip-address>:<port>
```

We can do DELETE request using something like this:
```sh
curl -X DELETE http://<our-public-ip-address>:<port>
```

We can also send cookie with something like this:
```sh
curl -b "name=bruhtus" -X PATCH http://<our-public-ip-address>:<port>
```

For more info, we can check `man curl` or `curl --help all`.

## Test

We can compare the value of variable with `test` command or `[ expression ]`.
For example:
```sh
[ -z $EDITOR ]
```
will check if the `$EDITOR` environment variable is empty string or not.

For more info, we can check `man test`.

## Array in Shell Script

Let's say we want to make an array of text editor, we can do that with
something like this:
```sh
# space indicate different item in an array, unless using quote which means
# that it's a single item despite there's a space in it.
editors=(vim nvim nano emacs "visual studio code")
```

Here's an example to access the item in `editors` array:
```sh
echo "Here's my favorite editors is ${editors[0]}"
```

And we can count the array length with something like this:
```sh
echo "We have ${#editors[*]} editors in the array"
```

## Do Arithmetic in Shell Script

We can do basic arithmetic in shell script with something like this:
```sh
# basically use dollar sign and double parentheses.
echo $(( 6 + 9 ))
```

## Cron

The idea of cron is that we have some tasks and we want it to be run at some
interval.

There's a few default for cron, you can check that by doing `ls /etc/cron.` and
then press <Tab>. You'll see something like this:
```sh
cron.d/        cron.daily/    cron.hourly/   cron.monthly/  cron.weekly/
```
and don't forget to make the file executable by doing `chmod +x <file>`.

> Keep in mind that our computer need to be running for the cron job to run.

### Crontab

If we need more defined schedule (like every five minutes, every other
Thursday, every six months, etc.) we can use crontab.

To use crontab, we can use something like this:
```sh
crontab -e
```

Inside the file, we write it something like this:
```sh
# `* * * * *` translate to: minute, hour, day of the month (25th day of the
# month), month, day of the week.
# to make things easier, use https://crontab.guru/
* * * * * <command-or-script-that-we-want-to-run>
```

## Customize Shell Prompt

We can check our prompt component with this command:
```sh
echo $PS1
```
and then we can edit the `PS1` variable like usual.

## References

- [Course website](https://btholt.github.io/complete-intro-to-linux-and-the-cli/).
- [Multipass website](https://multipass.run/).
- [Signal Linux manual page](https://man7.org/linux/man-pages/man7/signal.7.html).
- [Tutorialpoint about signal](https://www.tutorialspoint.com/unix/unix-signals-traps.htm).
- [Recursive acronym wikipedia](https://en.wikipedia.org/wiki/Recursive_acronym).
- [Crontab guru](https://crontab.guru/).
