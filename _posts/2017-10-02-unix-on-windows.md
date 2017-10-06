---
layout: post
title: Working on your PC
---

# BASH on Windows 10

If your personal computer (PC) operates using Windows 10, you should be able to install a windows application onto your system that allows the user to access and navigate the file system using an Ubuntu flavor of <a href="https://en.wikipedia.org/wiki/Bash_(Unix_shell)" target="_blank">BASH</a>. The procedure for outfitting your Windows 10 computer to operate with Ubuntu BASH terminal windows has been well documented in the following tutorial: [Install BASH for WINDOWS 10](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/){:target="_blank"}

# Git BASH

Because the Ubuntu Bash on Windows 10 is in its early __beta__ release version, we will not be using this tool in the labs this year. During the lab sections of the course we will be using __Git BASH__. If you would like to have consistency across the computer you use in lab and your personal computer, download and install *Git for Windows*, available at [https://git-for-windows.github.io/](https://git-for-windows.github.io/){:target="_blank"}.


### Macintosh Users

If you use a macintosh computer, the operating system is built on top of a unix frame. Mac OS X has a terminal application pre-installed. If you have never used the temrinal application on your macintsosh computer before, you can find it using __spotlight__ `command + spacebar` and type in 'terminal'. Alternatively, you can navigate to launching the application using __FINDER__ (Applications > Utilities > Terminal):

![]({{site.baseurl}}/images/terminal.png)


#### Other Useful Programs for Mac

* If customization is your thing, try out an alternative terminal emulator: [iterm2](https://www.iterm2.com/downloads.html){:target="_blank"}
* Popular Text Editors for Mac OS X:
  * [Sublime](https://www.sublimetext.com/3){:target="_blank"}
  * [TextWrangler/BBEdit](https://www.barebones.com/products/textwrangler/download.html){:target="_blank"}
  * [Atom](https://atom.io/){:target="_blank"}

## Using BASH with Caution

 If you have never used a unix-based operating system, or have limited experience, there are certain precautions that should be taken when navigating and manipulating files at the BASH command-line. Most importantly, __when a file is removed__ from the system using the `rm` command, the __file is gone forever!__ It does not go to a __"Recycle Bin"__ or __"Trash"__ Directory/Folder.

 Because of this, removing files within the BASH shell should be done with certainty. We recommend reading over [8 Deadly Commands You Should Never Run on Linux](https://www.howtogeek.com/125157/8-deadly-commands-you-should-never-run-on-linux/){:target="_blank"} in order to familiarize yourself with hazardous commands that will likely prove to be detrimental to your file system, should you run them.

 Perhaps the most significant stress point regarding using BASH with caution is to be careful when using the wildcard character, `*`, in conjunction with commands - namely the `rm` command.

 For example, you should never run: `rm /*` if you have Administrative access on the file system, commonly referred to as [SUDO](https://en.wikipedia.org/wiki/Sudo) permission in the unix world.

 ![]({{site.baseurl}}/images/sudo.gif)

 <small>[photo credit](https://www.pinterest.com/pin/370421138070787705/){:target="_blank"}</small>

### BASH Safeguard for the `rm` command

One safeguard when removing files at the command line is to add the `-i` option when calling on the `rm` command like so:

`$ rm -i <file to remove>`

This will prompt the interpreter to display a **warning** message in response to your call of removing the targeted file(s):

```
smhigdon$ rm -i foo.txt
remove foo.txt?
```

In response to this interactive prompt by the system, you may then respond by simply typing `y` or `n` for yes or no respectively, followed by the return key. If you types `n`, the file will not be deleted, whereas typing `y` deletes the file. You may think that this is a bit tedious, typing `-i` every time you want to safeguard deleting a file by mistake - it is! Luckily, there is a way to make the `rm` command safe by default, by embedding an `alias` into your `.bash_profile`. [See the lab page on the .bash_profile]({{site.baseurl}}/2017/10/05/bash_profile/){:target="_blank"}.
