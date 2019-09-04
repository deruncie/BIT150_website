---
layout: supplement
title: cheat-sheet of all the commands we saw in Lab 1
hidden: true
---

|   |               |
|---------|------------------------------|
| `whoami` | prints your username |
| `pwd` | **print working directory** prints the path to the current directory |
| `cd new_dir` | **change directory** changes to directory `new_dir` |
| `ls` | lists contents of current directory |
| `ls -F` | lists contents of current directory and annotates folders (`/`) and programs (`*`) |
| `man ls` | `man` is a program for viewing manuals (here of the `ls` program). Use `space` to page, arrow keys to move, and `q` to quit. In the SYNOPSIS, arguments in square brackets are optional. |
| `cd /` | go to the **root** directory |
| `cd ~` | go to the **home** directory |
| `cd .` | stay in the **current** directory |
| `cd ..` | go **up** one directory |
| `cd ../../` | go **up** two directories at once |
| `cd -` | go back to the last directory || `cat file` | print the contents of `file` to the screen |
| `> new_file.txt` | **redirect** command. Instead of printing to the screen, save output in the file `new_file.txt` |
| `>> old_file.txt` | **append** output to the end of `old_file.txt` |
| `mkdir new_dir` | creates the folder `new_dir` in the current directory |
| `mv my_file my_folder/` |**moves** file `my_file` to folder `my_folder` |
| `mv old_file new_file` | **renames** file `old_file` to `new_file` |
| `mv old_file my_folder/new_file` | **moves** file `old_file` to `my_folder` and renames it to `new_file` |
| `mv files* my_folder/` | moves all files beginning with `files` to folder `my_folder` |
| `ls files*` | lists all files that begin with the characters `files` |
| `history` | print the last 1000 commands to the screen |
| `bash my_script.sh` | runs each line of the file `my_script.sh` as if you had typed it into Bash directly |
| `variable=some_text` | assigns `variable` the value `some_text`. Note: no spaces around `=` |
| `$variable` | access the value held by `variable` |
| `echo $variable` | prints the value held by `variable` to the screen |
| `echo ${variable}.txt` | prints the value held by the variable followed by the text `.txt` to the screen |
| `# echo ${variable}.txt` | does nothing! `#` is a comment character, which tells Bash to ignore everything else on the line |
| `rm my_file` | deletes file `my_file` **immediately**. |
| `rm -i my_file` | asks before deleting |
| `rm -f my_folder` | deletes `my_folder` **and all its contents** immediately |
| `rm -if my_folder` | asks before deleting each file in `my_folder`, and then deletes `my_folder` |
| `cat My File.txt` | tries to print the file `My` to the screen (gives error: file not found). Then tries to print the file `File.txt` to the screen (another error) |
| `cat "My File.txt"` | successfully prints the file `My File.txt` to the screen. **Avoid spaces in file names!** |
| `head my_file` | prints the first 10 lines of `my_file` to the screen |
| `head -n 3 my_file` | prints the first 3 lines of `my_file` to the screen |
| `tail -n 3 my_file` | prints the last 3 lines of `my_file` to the screen |
| `wc my_file` | prints the #lines, #words, and #characters of file `my_file` to the screen. Options `-c`, `-w`, and `-l` select fewer stats |
| `sort my_file` |**sorts** the lines of `my_file` **alphabetically** by the first **word** in each line |
| `sort -n my_file` |**sorts** the lines of `my_file` **numerically**, interpreting the first word as a number |
| `uniq my_file` | compares each line of `my_file` with the preceeding one, and prints it to the screen if it is different |
| `cmd1 | cmd2` | run command `cmd1` and then take the output and get it to command `cmd2` to run immediately, avoiding any intermediate files |




### For loop syntax

```
for f in list
 do
  echo $f
done
```

`list` is any list of values, separated by a `space`. You can type: `file1 file2 file3`. Or you can use a wild card `file*`, or a sequence of numbers: `1 2 3 4 5`, or faster: `$(seq 1 5)`.

The for loop will:

1. take each value in turn, and assign it to the variable `f`: 
	- ex. `f=value`
2. Run the lines betwen `do` and `done`.
	- here, it runs `echo $f`; ie, it echo's the **value** of `f` to the screen
4. Goes back to the beginning. If there is another value in the list, it starts again at (1). Otherwise, it exits.








