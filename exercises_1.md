[Home](README.md)

# Lecture 1: Course Overview + The Shell

 1. > Create a new directory called `missing` under `/tmp`.
    ```bash
    ~$ cd /tmp
    ~$ mkdir missing
    ``` 
 1. > Look up the `touch` program. The `man` program is your friend.
    ```
    ~$ man touch
    ```
 1. > Use `touch` to create a new file called `semester` in `missing`.

    ```bash
    ~$ cd missing
    ~$ touch semester
    ``` 

 1. > Write the following into that file, one line at a time:
    > ```bash
    > #!/bin/sh
    > curl --head --silent https://missing.csail.mit.edu
    > ```
    > The first line might be tricky to get working. It's helpful to know that
    > `#` starts a comment in Bash, and `!` has a special meaning even within
    > double-quoted (`"`) strings. Bash treats single-quoted strings (`'`)
    > differently: they will do the trick in this case. See the Bash
    > [quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)
    > manual page for more information.

    ```bash
    ~$ echo '#!/bin/sh' >> semester
    ~$ echo 'curl --head --silent https://missing.csail.mit.edu' >> semester
    ```

 1. > Try to execute the file, i.e. type the path to the script (`./semester`)
    > into your shell and press enter. Understand why it doesn't work by
    > consulting the output of `ls` (hint: look at the permission bits of the
    > file).
    ```bash
    ~$ ./semester
    ~$ ls -l semester
    ``` 
    It doesn't work because it is not an executable file (it lacks permission for the 'x' bit).

 1. > Run the command by explicitly starting the `sh` interpreter, and giving it
    > the file `semester` as the first argument, i.e. `sh semester`. Why does
    > this work, while `./semester` didn't?
    ```bash
    ~$ sh semester
    ```
    It works because the file is passed as a script to the executable ``sh``.
 1. > Look up the `chmod` program (e.g. use `man chmod`).
    ```bash
    ~$ man chmod
    ```
 1. > Use `chmod` to make it possible to run the command `./semester` rather than
    > having to type `sh semester`. How does your shell know that the file is
    > supposed to be interpreted using `sh`? See this page on the
    > [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) line for more
    > information.
    ```bash
    ~$ chmod u+x semester
    ```
 1. > Use `|` and `>` to write the "last modified" date output by
    > `semester` into a file called `last-modified.txt` in your home
    > directory.
    ```bash
    ~$ ./semester | grep -i last-modified | cut -d ' ' -f 2- > last-modified.txt
    ````
 1. > Write a command that reads out your laptop battery's power level or your
    > desktop machine's CPU temperature from `/sys`. Note: if you're a macOS
    > user, your OS doesn't have sysfs, so you can skip this exercise.
    ```bash
    ~$ cat /sys/class/power_supply/BAT1/capacity
    ```   
