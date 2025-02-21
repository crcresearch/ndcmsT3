# Basic Linux Guide

All of the CRC HPC infrastructure is running some form of Red Hat Enterprise Linux. RHEL (Red Hat Enterprise Linux) is a Linux distribution, which is a flavor of a Unix-like operating system centered around the Linux kernel. To begin using the CRC HPC infrastructure, you will need a basic understanding of how to operate and navigate a terminal in a Linux environment. This page will show you the basics, but we highly recommend learning on your own some basic Bash commands.

The following is only intended to get you started, a more in depth guide can be found on the [Linux Journey](https://linuxjourney.com/) website.

------------------------------------------------------------------------

## Navigating File System

When first logging in, you will be placed into what is known as your Home space. This can be abbreviated with ~. Your prompt will most likely look something like this:

    [$USER@crcfe0X.crc.nd.edu ~]$ _

In your prompt you should see your ND NetID instead of `$USER`.

### Paths

A path is simply the location in a file system of a particular file or directory (folder). There are two different basic types of paths, absolute (fully qualified) and relative.

| Type | Def | Example |
|----|----|----|
| Relative | Path in relation to current directory | foo.txt |
| Absolute | Fully qualified unambiguous path | /afs/crc/user/u/username/research/foo.txt |

To see where you are currently in the file system hierarchy, also known as your **Current Working Directory** or `CWD`, can be found with using the `pwd` command:

    [crcuser@crcfe0X.crc.nd.edu ~]$ pwd
    /afs/crc.nd.edu/user/c/crcuser/

------------------------------------------------------------------------

### Viewing Files and Directories

To view which files and directories are in your Current Working Directory, simply use the `ls`:

    [crcuser@crcfe0X.crc.nd.edu ~]$ ls
    Private  Public  YESTRDAY

You can also pass a relative or absolute path to the `ls` command to view the contents of a particular directory without being within said directory:

    [crcuser@crcfe0X.crc.nd.edu ~]$ ls Public
    myprogram.py  mystuff  researchstuff  foobar.txt

You can change which directory you're currently within by using the `cd` command. You can also pass in a relative or absolute path into this command and it will behave as expected:

    [crcuser@crcfe0X.crc.nd.edu ~]$ cd Public
    [crcuser@crcfe0X.crc.nd.edu ~/Public]$ _
    [crcuser@crcfe0X.crc.nd.edu ~/Public]$ cd /afs/crc/user/c/crcuser/Private
    [crcuser@crcfe0X.crc.nd.edu ~/Private]$ pwd
    /afs/crc/user/c/crcuser/Private

------------------------------------------------------------------------

## Commands

In order to do anything within a terminal you will need to run commands. If you've been following along you already ran a few commands. The default shell for new CRC accounts is `bash`. Thus, you will be running bash commands for both UGE jobs and while logged into the front ends. If you have an older account (pre May 2018) your default login shell will most likely be `tcsh`. If you'd prefer to have either bash or tcsh as your login shell, feel free to drop us a line at <CRCSupport@nd.edu> and we'd be happy to change it over for you. If you are unsure what your login shell is, type `echo $0`.

To see a listing of basic commands, grab the sheet here: `Linux Coding Cheat Sheet <doc/fwunixref.pdf>`

------------------------------------------------------------------------

### Flags, Arguments, Options

Every command will have the name along with modifier flags, arguments, and options. These can drastically alter the result of the command or be more subtle. For example, to view hidden files you need to pass a flag to the `ls` command:

``` shell
[crcuser@crcfe0X.crc.nd.edu ~/research]$ ls -a
.  ..  .foo.txt  research.txt  foobar/
```

There are many, many flags to most commands. Normally, the `--help` flag is a standard for viewing a short help message about other options, but not always.

``` shell
[crcuser@crcfe0X.crc.nd.edu ~]$ ls --help
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                  [ message clipped ]
```

An argument to a command is simply something passed in via the command line to be harvested and used in someway by the command being ran. For example, the `ls` command can receive an argument of a path to list the contents of. Below, we are passing the argument of `~/research/python_scripts` to the command, which will then take that path and display the listing of contents. A command may do many different things with an argument, as there is no standard for what an argument is meant to do.

``` shell
[crcuser@crcfe0X.crc.nd.edu ~]$ ls ~/research/python_scripts
foo.py    helper.py   foobar.txt
```

Normally there is a manual page for a command, although this may not always be the case. A manual page will give you the technical details about running and utilizing a certain command in terms of options, bugs, flags, and more. The command to view a manual page is `man COMMAND_HERE`. This will normally bring up a pager such as `less` to view the page, where you can scroll up and down using the arrow keys, search for strings and more, or scroll with "J + K" keys. To exit the man page, you can press the "Q" key.:

    LS(1)                                                                                           User Commands                                                                                           LS(1)



    NAME
           ls - list directory contents

    SYNOPSIS
           ls [OPTION]... [FILE]...

    DESCRIPTION
           List information about the FILEs (the current directory by default).  Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

           Mandatory arguments to long options are mandatory for short options too.

           -a, --all
                  do not ignore entries starting with .

           -A, --almost-all
                  do not list implied . and ..

           --author
                  with -l, print the author of each file

           -b, --escape
                  print C-style escapes for nongraphic characters

           --block-size=SIZE
                  scale sizes by SIZE before printing them; e.g., '--block-size=M' prints sizes in units of 1,048,576 bytes; see SIZE format below

------------------------------------------------------------------------

### FS Manipulation

Organizing files and directories can free up time normally spent searching for files. If you'd like to create a directory to hold data etc for a certain portion of research, you can do so with `mkdir`. Let's say you'd like to rename a directory or file, you can use the `mv` command to move that file / directory to a different name.:

    [crcuser@crcfe0X.crc.nd.edu ~]$ mkdir research
    [crcuser@crcfe0X.crc.nd.edu ~]$ ls
    research  Public  Private  YESTRDAY
    [crcuser@crcfe0X.crc.nd.edu ~]$ mv research malaria_research
    [crcuser@crcfe0X.crc.nd.edu ~]$ ls
    malaria_research  Public  Private  YESTRDAY

Copying files is another important task which can easily be accomplished. First you'll need to consider if you are copying a single file or a directory. To copy a single file, simply use `cp src dest` where src is the original file you'd like to copy and dest is the destination (this can include a path either relative or absolute). If you're copying a directory, you'll need to pass the `-r` flag to indicate you'd like to recursively copy files from said directory.:

    [crcuser@crcfe0X.crc.nd.edu ~]$ ls -F
    foo.txt  run.py*  research/
    [crcuser@crcfe0X.crc.nd.edu ~]$ cp foo.txt bar.txt
    [crcuser@crcfe0X.crc.nd.edu ~]$ ls -F
    foo.txt  bar.txt  run.py*  research/
    [crcuser@crcfe0X.crc.nd.edu ~]$ cp -r research test_research
    [crcuser@crcfe0X.crc.nd.edu ~]$ ls -F
    foo.txt  bar.txt  run.py*  research/  test_research/

------------------------------------------------------------------------

### Opening Files

Opening files within the terminal is normally the most efficient way to quickly see the results of some runs or check on output etc. There are several ways to see the contents of a file, with different methods being ideal in different situations. To simply dump the contents of a file to the screen, you can use `cat`. Be careful doing so, as even if the file is a binary (compiled executable) or raw data file it will still dump gibberish to your screen. If the file is long and you'd like to easily scroll back and forth, you can open the file with a pager like `less` or `more`. Note that these will seemingly take over your terminal and you'll need to exit them to get back to the cmd prompt. Normally this is as easy as pressing the "Q" key.:

    [crcuser@crcfe0X.crc.nd.edu ~]$ ls -F
    foo.txt  run.py*  research/
    [crcuser@crcfe0X.crc.nd.edu ~]$ cat foo.txt
    this is text from the file foo.txt
    this is more txt. I am text.

### Editing Files

Editing files from a terminal can be done in many ways. You can simply edit the file and transfer the edited file to the CRC servers, or edit in place through the terminal. There are several terminal text editors with varying degree of learning curves. Below they are organized from easiest to learn to hardest, normally the more difficult to learn indicates a more powerful editor in terms of features and abilities.

- [nano](https://www.tecmint.com/learn-nano-text-editor-in-linux/)
- [emacs](https://www.gnu.org/software/emacs/tour/)
- [vim](https://www.linux.com/tutorials/vim-101-beginners-guide-vim/)
- [ed](https://www.howtoforge.com/linux-ed-command/) -- Old editor, exists but we do not recommend.

Typically with each above, you can create a file for example vim newfilename.txt, or you can edit an existing file with the same command. Learning the ins and outs of a particular editor can take some time, but is almost always worth the effort to save time transferring files when a small change is needed. Most editors have the ability to customize the operation, look, and feel through configuration scripts and can add different plugins for additional modifications. It is not unheard of for some programmers to use one of the above editors as their preferred text editor for everyday use.
