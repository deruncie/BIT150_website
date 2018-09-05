---
layout: supplement
title: .bash_profile and the shell ENVIRONMENT
hidden: true
---

# What is a .bash_profile?

The `.bash_profile` is a plain text file that is sourced by the login shell when opening a new terminal window.
Another file that can be sourced is the `.bashrc` file.
For more information on the difference between these two, see [here](https://apple.stackexchange.com/questions/51036/what-is-the-difference-between-bash-profile-and-bashrc){:target="_blank"}. These files are hidden in the home (`~`) directory, which is achieved by placing a `.` at the beginning of the file name. To view all hidden files within a directory, type `ls -a`. This will reveal hidden files. If your home directory does not have a `.bash_profile` already, create one:

```
$ touch ~/.bash_profile
```

These files are used to customize the environment of your bash terminal. When opening a new login shell window, the terminal shell program will look in the home directory for the `.bash_profile` file, read the file line by line, interpret the commands and apply them to the terminal shell's environment. __Each bash terminal window environment is independent from those of other windows__.


## Alias
A common type of environmental modification to store within the `.bash_profile` is an `alias`. An `alias` is used to modify the default behavior of a linux command, such as `ls` or `rm`. Incorporating an alias into your .bash_profile file could be done like so:

```
alias rm='rm -i'
```

Setting up this alias in the `.bash_profile` will setup your terminal application to spawn a bash environment where typing `rm` at the command-line is equivalent to typing `rm -i`, every time you open a new terminal window.

## Path
A second factor to consider for the `.bash_profile` is appending directories to your shell environments `PATH` environment variable. To see what your environment variable is set to by default, type:

```
$ echo $PATH
```

The directories returned in the terminal window are all places that the bash shell will search for binary files that are used to invoke programs within the environment. When the installation of a new piece of software is desired, one that you wish to run from the command line, the directory in which the files are stored must be communicated to the bash interpreter. This is done by adding the target path to the `.bash_profile`. Here is an example of how to append a directory to your environment path:

```
export PATH=$PATH:$HOME/BIT150/scripts
```

Another way to achieve the same thing is:

```
export PATH="~/BIT150/scripts:$PATH"
```

Adding one of the lines above to your `.bash_profile` will tell **bash** to look in `~/BIT150/scripts` for executable files that match the command specified at the command line prompt. These can be __shell scripts__, __python scripts__, __R scripts__, __perl scripts__, etc.

## Comment Lines

As discussed in [Lab2]({{site.baseurl}}/2017/10/05/lab-02/){:target="_blank"}, adding comments to files that are interpreted by the shell is extremely important. This allows you to communicate to your future self, as well as others who may view the file you create. Comments may be added in the line above lines of code that are intended to be read by the shell, as well as on the same line that the code is being written on:

```
# Aliases

alias rm='rm -i' # this is an alias to make the rm command safer.

```

## Example of a simple `.bash_profile` file

```

###############################
#                             #
# .bash_profile for bit150-01 #
#                             #
###############################

# Welcome and Date

MYDATE=`date "+%H:%M:%S %m/%d/%y"`
echo "Welcome $USER, the current time is $MYDATE"

# ENVIRONMENT ALIASES

alias ls='ls -p' # show which items are directories by default

alias rm='rm -i' # make remove command safer by adding -i option

# ENVIRONMENT PATH

# add the BIT150/scripts directory to the ENVIRONMENT PATH
export PATH="/Users/smhigdon/BIT150/scripts:$PATH"
```

Once your `.bash_profile ` has been created in the home directory, you need to tell your active shell to read it:

```
source ~/.bash_profile
```

All new shell windows that you spawn after saving this file into the home directory will source the .bash_profile - or each time you login to farm.

### Making a file executable
To make a script file you have stored in `~/BIT150/scripts` executable, use the following strategy:
```
chmod u+x <path to script file>

e.g.

chmod u+x ~/BIT150/scripts/my-first-script.sh
```

To verify that the file has been made executable, type:

```
smhigdon$ ls -l ~/BIT150/scripts/my-first-script.sh


-rwxr--r--  1 smhigdon  staff  0 Oct  5 18:09 BIT150/scripts/my-first-script.sh
```


The x indiates that the file is now executable.

Now, with the .bash_profile created and the script file made executable, you should be able to run the `my-first-script.sh` program from any directory because the shell searches the scripts location for any executable file.
