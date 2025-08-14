```bash
#!/bin/bash

touch comp206.c
echo "#include<stdio.h>" >> comp206.c

echo "int main(void){" >> comp206.c
echo "	printf(\"COMP 206 - Introduction to Software Systems\\n\");" >> comp206.c
echo "	return 0;" >> comp206.c
echo "}" >> comp206.c

gcc -o "comp206" comp206.c
./comp206
rm comp206.c
```

This course is an introductory course to development within software systems. It introduces a variety of tools that are used throughout the field such as software development techniques, the Linux operating system, and the C programming language. It builds on the foundation from previous computer science courses and prepare students to be able to work in large scale development teams and low level software systems.

The course primarily uses Bash and C, which are taught during the course.

This course has either [COMP 250 - Introduction to Computer Science](./comp250.md) or [COMP 202 - Foundations of Programming](./comp202.md) as prerequisites.

### Course Content

- [**Unit 1**: Unix](#unit-1-unix)

Unix based operating systems (primarily Linux)

- [**Unit 2**: Bash](#unit-2-bash)

Bash Scripting

- [**Unit 3**: C ](#unit-3-c)

The C programming language

- [**Unit 4**: Developer tools](#unit-4-developer-tools--techniques)

GNU tools, Modular Programming, Team workflow, Git

- [**Unit 5**: Systems programming](#unit-5-systems-programming)

Introduction to Systems programming

# Unit 1: Unix
> Note: This course is 25% theory and 75% about how to apply theory. More than any other COMP course thus far, you will need to practice on your own if you want to learn what this course is attempting to teach you

## Introduction to Systems

Before we look at Linux, let's first overview what a system is.

A **system** is a set of interconnected components that interact with each other. Each component performs a specific function. Example of systems include transportation systems to move people around the world, and government systems to figure out how to crash the economy as hard as possible. Here, we're going to focus on software systems.

Software is typically divided to two main types. **Application Software** is software designed to be used by the user, while **systems software** is designed to be used by other software. Think of an application vs the libraries used to make that application.

So far, you are assumed to have taken COMP 202, and maybe COMP 250. Both these courses tackle high level concepts to design algorithms to handle data and tackle big problems. This course will dive more into the lower level programming: closer to the operating system, but just above hardware level.

### Operating Systems

An **operating system** (OS) is a piece of software that allows us to interact with a computer without needing to know how the inside of a computer works. Its role is to manage the computer's resources and allow us to safely interact with those resources. The OS allows us to control the computer and manage things for us.

You currently own a computer that is running an operating system. Ignoring mobile phones, statistically, 70.62% of you have a Windows computer, 15.74% of you use MacOS, 8.01% are so special we don't even know your operating system, and 3.81% of you use a distribution of Linux[^1].  For this course, you will primarily use the Linux Operating System.

[^1]: _Desktop operating system market share worldwide | StatCounter Global Stats_. (n.d.). StatCounter Global Stats. https://gs.statcounter.com/os-market-share/desktop/worldwide/#monthly-202502-202502-bar

## Linux

[^ Back to top](#table-of-contents)

Linux is a family of open source UNIX based operating systems that are based on the Linux kernel. Originally developed by Linus Torvalds in 1991, the Linux kernel has since grown into the kernel of choice for many of the software infrastructure across the world due to its free open source nature, performance, stability and ecosystem of open source applications.

If you are currently enrolled in the course, you will probably need to know how to connect yourself to a remote linux server. Unfortunately, to connect to your account on a remote server, you need an account on a remote server, which you won't have if you're not actually enrolled in your university course. If you do have one though, you will have to `ssh` your way into your account with the following command

```
ssh your_username@your_serverhost.ca
```

If that's successful, you will most likely be prompted to enter your password. You'll notice at this stage that if you try to type anything, nothing will be displayed on screen. This is intentional to improve the security and make it harder for anyone to get your password by typing it.

Type in your password and enter. You are now in a linux kernel

### Common linux shell commands

First of all, where even are you? If you did all of the previous, you are probably in your **home directory**. You are probably familiar with folders on your personal computer. For terminology purposes, these are named directories on the command line. Your personal account's files start at the home directory, represented by a tilde `~`. Here are other directory shortcuts that linux employs

- `/` is the root directory. This is the "directory" where everything on your computer is installed, including the operating system
- `~` is the home directory. This is the directory where all your personal files reside.
- `./` indicates the current directory.
-  `../` indicates the parent directory (the previous one)

With that, we can take a look at our first linux command. Change directory `cd` allows us to go from one directory to another. Here are usage examples

```
cd ./a_directory 
cd ../a_directory_in_the_previous_folder
cd /
cd ~/a_directory/another_directory
cd homework
```

A `path` is a string that expresses the "route of directories to take" to reach a certain file. There are two types of paths.

- Absolute paths is a path that starts at the root. For example `/dir/dir/file`
- Relative paths is a path that starts at the current location. For example `./dir/dir/file`. Note that when writing relative paths that go to a file/directory in the current directory, the `./` is unnecessary. We can therefore rewrite our relative path into `dir/dir/file`

You probably want some directories to actually try out this new `cd` command. Let's `mkdir` some new directories.

```
mkdir a_new_directory
```

Now we made a new directory. How do we know. Let's list directory's components using `ls`

```
ls
```

Do you see your newly created file?

Some other usage notes about `ls`.  You can add a directory name to list a specific directory's contents

```
ls some_dir
```

You can also add *flags* to the `ls` commands. There are a lot of flags, but here are the most useful ones.

- `-a` lists everything, including hidden files/directories (hidden files/directories start witha `.`)
- `-d` only lists directories
- `-l` long listing format. Combine with the `-h` flag to make it more readable

Let's look at what gets printed when you use `ls -l` and go over every element one by one

```
drwxrwxr-x 2 user group 4096 Aug  3 19:48 a_dir
-rw-rw-r-- 1 user group   12 Aug  3 19:48 a_file.txt
-rwxrw-r-- 1 user group   31 Aug  3 19:49 some_executable.sh
```

- The `drwxrwxrwx` string is the permission string. There are 4 segments. 
  - The `d` indicates whether or not it's a directory. 
  - The three `rwx` indicates **r**ead, **write** and e**x**ecute permissions for three group of users: `user` which is the person who owns the file, `group` which is the group of users that's part of the same group as the owner, and `others` which is everyone else. 
  - The permission is represented by its letter if a group has it, and a `-` if a group doesn't have that permission
  - To modify permissions, you may use the `chmod` command. Example `chmod` command usage are `chmod u+x` to give owner execute permission, `chmod o-r` to remove others read permission,`chmod g=rx` to give group only read and execute permissions. 
- The number next to that string represents the number of *hard links* the file/directory has.
- `user` indicates the name of the user who owns the file
- `group` indicates the group the user is a part of
- The number next to `group` indicates the file size in *bytes*. If you used the `-h` flags, it converts it to the nearest SI prefix
- The *date* and *time* area is the time the file was last modified.
- The *name* is the name of the file

### List of basic linux commands

Now let's quickly go over other useful linux commands related to files and directories
- `pwd` prints the working (current) directory
- `rmdir [options] directory` removes the directory with the name inputted to it
- `touch [options] file` creates a new file with the given name
- `cp [options] file1 file2` copies `file1` into `file2`. If `file2` already exists, it is overridden
- `mv [options] file1 file2` moves the contents of `file1` into `file2`. If `file2` already exists, it is overridden
- `rm [options] file` removes a file permanently.
- `cat [options] file` concatenates a file and displays the result (basically, displays a file's contents)
- `less [options] file` page through a file

And here's a list of options for the `cp`, `mv` and `rm` commands specifically
- `-i` interactive: creates confirmation prompts that need to be approved before proceeding
- `-r` recursive: recursively visits a directory
- `-f` force: removes all confirmation prompts (overrides `-i)

> You've probably read at some point to run the command `rm -fr /` to delete the french language pack or something in those lines. What this command actually does is recursively remove your root folder without prompting for confirmation. This deletes every single file on your computer, including the operating system. Basically, don't run that.

Here are some symbols that redirect the output of a command elsewhere

- `command > file` output of `command` is written into a new file called `file`. If `file` already exists, it's overridden
- `command >> file` output of `commmand` is appended to the end of and existing file called `file`. If `file` doesn't exist, creates a new file called `file`   
- `command1 | command2` output of `command1` becomes the input of `command2`. Particularly useful with certain commands that find elements within some text

To get more information about any of the commands, use the manual `man command` command to read documentation about any command. Do `man man` as a test

### Wild cards

Let's imagine for example you have a folder containing various files of different types: `.png` files, `.mp4` files, `.txt` files, `.doc` files. You want to copy all `.txt` files into a new directory. How do you do that? You could manually copy every single `.txt` file you see into that new directory. That's incredibly inefficient, and if you have hundreds or thousands of files, you're bound to miss a couple. Instead, let's take advantage of **wildcard symbols**.

**Wildcard symbols** are symbols used to replace one or multiple characters in a file name. In Bash, we have
- `*` asterisk replaces any pattern
- `?` question mark replaces any single character
- `[]` square brackets replaces with any character within its square brackets. For example `[abc]` will be replaced with a,b, or c

Let's look back in our previous example. We can now do the following command

```
cp *.txt some_dir/*.txt
```

And now, all our `.txt` files got copied over. Nice

### Regex

There exists more text patterns that several Unix commands allows to search more advanced text patterns. These are called **regular expressions** or *regex*. These are absolutely hell to both know and use, so I will just link [this regex cheat sheet](https://www.rexegg.com/regex-quickstart.php) and wish you good luck

There are 3 popular commands that use regex. Let's look at `grep [options] string file_list` first.

This command searches for all occurences of the string `string` within a file. For example, if we have a file `fruits.txt`

```
Apple
Banana
Blackberry
Blueberry
Clementine
Cranberry
Dragonfruit
Grapes
Lemons
Mango
Oranges
Passionfruit
Raspberry
Strawberry
```

Then using the command

```
grep 'Bl' fruits.txt
```

will return the following

```
Blackberry
Blueberry
```

This is quite useful as not only can `string` of `grep` use regular expressions, but you can also redirect a command's input into `grep`. Feel free to try out (and cry about) regex more on your own.

## Vim

Ok, you've probably toyed around with linux directory and file creation for quite a while now, but now you want to start writing some files. How do we do that?

Well you could always use redirection to `echo "line of code" > file` write all your code. That will only be somewhat impractical, very annoying to edit, and overall a horrible idea. Instead, let's use a text editor.

Linux has a lot of text editors that work with its kernel. Some of them include `vi`, `emacs` , and`nano`. All of them have cult like followings for each of them and very dedicated communities. The one we'll be using in this course is `vi`, or more specifically `vim`

For a bit of history, `vi` is a text editor released in 1976 by Bill Joy as a visual text editor for early Unix systems. This was still at a time when the mouse didn't exist for computers so it was designed to function exclusively with the keyboard. In 1991, vi-improved, or `vim` was released. This improved `vi` by adding much liked features such as syntax highlighting for programming languages, and the ability to customize your editor with plugins. Today, most linux distros have `vi` pre-packaged, and most linux systems have `vim` installed.

> To continue the history a bit further: in 2015, Neovim was released as a fork of Vim. It is 99% identical to vim, but improved significantly on the extensibility and maintainability of vim.

Vim is a command-line editor. This means that you open it and work exclusively in the command line with it. Although not a proper IDE, it's still incredibly popular within developers, and has been ranked 5th most popular development environment amongst developers.

### Getting started with vim

To get started with vim, simply type `vim` in the command line. This will either open up the `vim`, or prompt you to download vim with your package manager.

Immediately upon entering vim, you'll be entered into **Normal mode**. Vim has 4 different main modes, although you will most likely only use 3 of them

- **Normal Mode**: Default mode. Re-enter normal mode by pressing `Esc` at any time. 
- **Insert Mode**: Enter insert mode by pressing `I` while in Normal mode. This will allow you to write text similar to most modern editors.
- **Visual Mode**: Enter visual mode by pressing `V` while in Normal Mode. Allows you to select areas of text. Anecdotally though, I've never used Visual mode
- **Command Mode** Enter command mode by pressing `:` while in Normal Mode. This will open a command line that allows you to write any vim command.

Vim is a keyboard based text editor, meaning that by default, the mouse does nothing (remember that `vi` was designed before the mouse existed for computers). There's an endless amount of keybinds and shortcuts that vim has. The best (and pretty much only) way to learn all these keybinds is to use vim and practice them yourself. You can access a cheat sheet of all the shortcuts [here](https://vim.rtorr.com/), an interactive tutorial for vim [here](https://openvim.com/), and also use the built-in `vimtutor` command on your command line that opens a document in `vim` designed to teach you the most important keybinds in vim.

### Customizing Vim

With that in mind, Vim functions like any other text editor. However, in its base form, you probably wouldn't want to use vim to do any coding, as it barely has any functionalities that make it different to coding in notepad.

This is where you would want to start customizing vim. There are an endless amount of customization you can give to vim. Some even require downloading them from the internet via a package manager. For now, let's look over a few you can do with built in features. You can type all of these commands in command mode

- `syntax on` turns on syntax highlighting for your code
- `set number` turns on line numbers in your text file
- `set tabstop = x` sets the tab character size to be `x` spaces. Useful for readibility.
- `set ignorecase` ignores capital letters when using any of the search functions.

If you want to go over a list of customizations for vim, you can find many online. You may even create a `.vimrc` file in your home directory to include a vim profile that will automatically be loaded whenever you open vim.

### Exiting vim

To exit vim, return to normal mode by pressing `Esc`, and type in command `:q` to quit. If you have made changes and would like to save them, type `:w` to save to the existing file, or `:w file_name` to save to a new file. Use `:q!` if you want to discard any changes. Use `:wq` as a shortcut to save and quit vim.

Congratulations, you have successfully done [what over 3.2 million people have struggled to do](https://stackoverflow.com/questions/11828270/how-do-i-exit-vim).

## Other commands and concepts

This section will just be listing a bunch of other commands and concepts that didn't fit in with the others.

There are various file types that developers use to archive various files together. 
- A **file** is any document on your disk's storage space
- A **tar file** is a file ending in `.tar` that contains a collection of files in a single document
- A **zip file** is a file ending in `.zip`, that contains the same information as a file, but uses less storage
- An **archive file** is a collection of zip files stored in a single document

There are many archiving formats. For now, let's look at the `.tar` format

You can use the `tar` command to create, update, and extract `tar` files. A file ending in `.tar` is an archive file, while a file ending in `.tgz` is a compressed tar archive file.

Here are other commands

- `diff file1 file2` allows you to compare two files and highlight differences
- `ln` allows you to create links between files. A link to a file will redirect to the original file whenran
- `sort [options] file` lets you sort the lines of a file
- `wc [options] file` displays a word count in a file

# Unit 2: Bash

[^ Back to top](#table-of-contents)

> This unit will be quite short, as much of it is an extension of the previous unit

In this course, we'll be looking at two primary coding languages: how to script using bash, and how to program using C. What is the difference between the two? Here are the primary ones

| Bash                                   | C                                        |
| -------------------------------------- | ---------------------------------------- |
| Scripting language                     | Programming language                     |
| Interpreted, Slow                      | Compiled, Fast                           |
| Primarily used on automating processes | Primarily used to build complex software |
| Simpler syntax                         | More complex syntax                      |

## Bash scripts

So far, all that we've done with the terminal has been done in the *bash shell*. We can now create *bash files* to group many of these commands together. A **bash file** is created with either the `.sh` file extension, or no file extension, and is frequently used for automated tasks in a Linux environment.

If you create a script file, there are two ways to run it. You can first call the interpreter by command and redirect it to the file

```
bash script.sh
```

This will run the script file `script.sh`. You can also use what's called a **sha-bang** `#!`. This will tell your shell which interpreter to use to run your script. You just need to put it at the top of your file. For bash, the sha-bang takes the following format

```
#!/bin/bash
```

and now, with the sha-bang, we can run our script file by calling its path

```
./script.sh
```

As another example, python is another very popular interpreted language and often used for scripts. In Linux, you can use the `python3` interpreter to run python files the same way

```
python3 script.py
```

and use the following sha-bang in your python files

```
#!/bin/python3
```

Now create a new bash script using `touch script.sh`, and add the sha-bang. We can now start scripting

### Bash commands

Bash scripting syntax is quite different from all other languages you may have had before. Similar to Python, semi colons are optional here. Unlike Python however, bash doesn't use indentation to define code blocks.

Let's start with commands. Every shell command you have used thus far is also a valid bash command. We have ran everything so far in the bash shell after all. To print hello world, simply write

```bash
echo "Hello World!"
```

Every other command also works the same way as it did in the shell

## Variables

To declare a variable in bash, we assign a name to a value

```bash
x=10
y="Hello World!"
```

> Notice how we didn't put spaces between the assignment operators. White space placements in bash scripts is incredibly delicate.

To get a variable's value, we use `$` before the variable name

```bash
echo $x  #prints 10
echo $y  #prints Hello World
```

> Comments are written with a `#`, similar to python

You can use the `local` keyword to declare a local variable

```bash
local a=1.3
```


### Special parameters

You can execute a bash script using certain parameters.Think of a sum script where you input two numbers to get a sum. You want to be able to do

```
./sum.sh 2 3
```

where this script outputs 5.,

To do so, you may use one of several special parameters bash employs

- `$#` returns the number of arguments in the command line
- `$-` returns the number of options supplied to the shell
- `$?` saves the exit value of the last command executed
- `$$` returns the process number of the current process
- `$!` returns the process number of the last command done in the background
- `$n` returns argument at position `n` where 1 is the first argument from the left, 2 is the second, and so on. (for two digit values, you must use `${10}`, `${11}`, etc.)
- `$0` returns the name of the current program (or shell)
- `$*` returns all arguments on the command line as a string
- `$@` returns all arguments on the command line as an array

### Quotation marks

The following is a list of quotation marks and brackets used to define text

- No quotation marks leaves it up to the shell version
- Single quotation marks `' '` leaves the string as is, the way it's typed in
- Double quotation marks `" "` first processes (formats) the string 
- You can put commands between backticks `` ` ` `` to capture their output first to write as a string. You can also use brackets `$(command)` to the same effect
- You can use curly brackets `${}` to concatenate characters. For example, if you have `animal='Cat'` and `animals='Mammals'`, if you want to print out `Cats`, you can do `echo "${animal}s`
- You can use two brackets `$((expression))` to evaluate an expression. For example, if you want to print the result of 1+1, you would do `echo "$((1+1))`. Just doing `echo "1+1"` doesn't work

## Control structures

The following will be all about how to make various program flows in bash

### Conditional Statements

To test for a condition, we write

```
if [[ Expression ]] 
```

There are a ton of tests that we can do, here are a list of some of them

For files `if [[ -r $file ]]`

- `-r file` returns true if file exists and is readable
- `-w file` returns true if file exists and is writeable
- `-x file` returns true if file exists and is executable 
- `-f file` returns true if file exists and is a regular file
- `-d file` returns true if file exists and is a directory

For strings `[[ -z $string ]]`

- `-z string` returns true if the string is length zero
- `-n string` returns true if the string is length non-zero
- `string1 = string2` returns true if strings are identical
- `string != string2` returns true if strings are not identical
- `string` returns true if string is not NULL

For integers `[[ $n1 -eq $n2 ]]`

- `n1 -eq n2` returns true if n1 and n2 are equal
- `n1 -ne n2` returns true if n1 and n2 are not equal
- `n1 -gt n2` returns true if n1 is greater than n2
- `n1 -ge n2` returns true if n1 is greater or equal to n2
- `n1 -lt n2` returns true if n1 is less than n2
- `n1 -le n2` returns true if n1 is less or equal to n2

### If/else statements

To write an if/else statement chain in bash

```bash
if [[ condition ]]
then
	_code_
elif [[ condition ]]
then
	_code_
else
	_code_
fi
```

> The keywords are important, as bash uses those to determine when a code block starts or ends

### Case statement

Similar to a java switch statement

```bash
case condition in

	condition1) action1;;
	condition2) action2;; 

	*) else_action;;
esac
```

### Loop statements

The for loop is an iterator through a list in bash. Similar to a *foreach* loop

```bash
for var in list
do
	actions
done
```

The while loop works like a traditional while loop

```bash
while condition
do
	actions
done
```

You can use the `continue` and `break` statements very similar to how you do it in Java.

### Functions

To declare and call a function in bash, you do the following

```bash
foo() {
	commands
}

foo
```

For example, here's a function that calculates the sum of two integers and prints it

```bash
sum() {
	local x=$1  #Use the local keyword to declare a local variable in a block
	local y=$2  #Positional parameters to use during function call
	
	echo $(( $x + $y ))
}

sum 2 3 #Prints out 5
```

You cannot return values with functions in bash. If you want to take a value from your functions, you can use work arounds such as using command substitution `$()` or using a global variable.

Finally, a quick note about `exit`. This instantly terminates the script. You can feed an integer after which will then become the exit code.

And that's it for bash. There are more advanced topics such as advanced commands. But I won't be covering them here. I'll list them and you can discover them by yourself.

`find`, `sed`, `awk`, `crontab`

# Unit 3: C

[^ Back to top](#table-of-contents)

> Ok, unlike bash, we have a lot more content to go through in this unit, so buckle up.

So while bash is very useful for automation scripts, it's not as useful for making most of the software systems you are using due to its interpreted nature and various features it lacks. You've probably already seen either Python, Java, or both, which are both very popular languages for various systems. In this course, we will be looking at C.

C is a high-level general purpose programming language invented in the 1970s. It is the language behind many of the software infrastructures you use today. To list a few, your operating system was coded in C. If you ever used Python, its interpreter was made in C. If you used Java, its syntax is heavily inspired by C. Vim, which we saw earlier, was coded entirely in C. Git, which we will see later, was also coded in C. It powers many of the low level infrastructure in software, which is why it remains a popular and influential language today.

Although C is classified as a high-level language, it does not have many of the high level features that modern high-level languages feature. This means that you have to do a lot more manually in C, and that it's a lot closer to low level programming than many of the other languages you've seen

I will assume you know either Java or Python, meaning I will assume you know the basic concepts that apply to each. I will not make assumptions you know either one, meaning I will not make references to either language. Ready? Let's start

## Compiling a C program

So here's an example C program

```C
#include <stdio.h>

int main(void){
	printf("Hello World!\n");
	return 0;
}
```

Each part will be explained in detail later. For now, just know that what you are looking at is the **source code**. This is the code that the programmer writes, and is the version of the code that's actually readable to everyone (to some extent).

So far, if you used Python, you're used to the Python interpreter, which interprets your code "in real time". If you used Java, you might've compiled your source code into class files that were interpreted by the JVM, or you most likely skipped the compilation and went straight to the interpretation with your IDE.

C will be the first programming language that you will have to manually compile before trying to run your program. C code is directly translated into machine code from your source code that is designed to be run on specific architectures.

To compile a C file, you must first install a C compiler. There are many to choose from, but we will be using the **G**NU **C** **C**ompiler (gcc). If you are using a device owned by your school, it's likely already installed. Otherwise, you will have to install it using your package manager.

> You may also install an IDE to make development less painful. However, it is entirely possible to use Vim to write C code as well

Now assuming our earlier source code was in a file called `hello.c`. To compile our program, we run

```
gcc ./hello.c
```

This will create a file called `a.out`. We can execute this file

```
./a.out
```

to get our output

```
Hello World!
```

There are many options we can specify to the compiler. For now, we will look at the `-o` flag, which allows us to specify the output program's name. Therefore, if we run

```
gcc -o helloworld.program hello.c
```

We will create a file called `helloworld.program` instead that does the same thing.



## Characteristics of C

This section will look at characteristics of the C language, and highlight differences between it and high level languages you are familiar with.

### Structure of a C file

Let's look at our Hello World program again

```C
#include <stdio.h>

int main(void){
	printf("Hello World!\n");
	return 0;
}
```

A C file is comprised of three parts: the `include` statements, the functions, and the main function.

The `include` statements import a collection of built-in functions called the Standard Library. There are 29 header files inside it, most of which we will see later. `<stdio.h>` is the standard input and output library, which include many functions relating to input and output, including `printf()`. You can also include your own header files, which we will look at later.

After our include statements, we have functions. We will look at functions more later, but for now, let's look at the main function

The main function is where the code will start executing when you run the machine code file. The `int` specifies the return type, meaning our main function returns an integer. Code found outside of a function will return a compiler error.

Important note about the main function is that it can take parameters that represent command line arguments. To do so, we have to add parameters to the main function

```C
int main(int argc, char* argv[])
```

where `argc` is the number of arguments, and `argv[]` is an array of strings representing the arguments' values.
### C syntax

C uses curly brace syntax. Code blocks start and end with curly braces `{}`, and you have to terminate every statement by a semicolon.

Indentation is optional, but it's good practice to have them consistent to avoid unreadable code. C is already hard enough to read as it is.

Comments in C have two forward slashes `//` before them. You can also make block comments with `/* */`

> If you're familiar with Java, you're already familiar with this style of syntax. If you come from Python... you'll get there, don't worry.

### Functions & Variables & data types

C is a statically typed language. This means that you have to declare a variable's type when declaring a variable, and a variable cannot take on other types. The same goes for functions, as you need to declare a function's return type

At its base, C has four basic types:
- `char` or character, represented by an integer by ASCII conventions
- `int` or integer, at least 16 bits, but usually 32 bits
- `float` usually 32 bits, with 6 digits precision
- `double` usually double the size of a float, allows more digits in its range with more precision

You may have noticed two particularities in those data types
- There are no `boolean` types
- There are no string types

C does not use booleans by default. All its comparison functions will instead take in or return an integer `1` or `0`. `1` is equal to `true`, while `0` is equal to `false`

C also does not have strings by default. It instead uses an array of characters to form strings. You can create an array of characters easily with the double quotation marks `" "`, which will generate an array of characters that has a null character `\0` at the end. This is what we did with the Hello world example earlier.

For `int` specifically, you can add modifiers to it. You can add the word `signed` or `unsigned` to tell whether your integer can represent negative numbers or not. You can also add `short` to give less bits to store your int, or `long` to give more bits to store your bits (typically 16 short, 32 normal, and 64 long). An int is signed by default.

> Unsigned and signed also works with char. By default, a char can be either signed or unsigned

There is also the `void` value. It specifies nothing

Here are example variable declarations and initialization and example functions

```C
float someFunction(int a, float b){
	float c = b + (float) a;
	return c;
}

int main(void){
	int userAge;   // Declared a variable, but not initialized
	float temperature = 23.5;  //Declared and initalized the variable
	char bestLetter = 'E';
	char helloWorld[] = "Hello World!";
	double pi = 3.141592653589793238462643383279502884197;
}
```


> You can cast a value from one type to another using the brackets as shown above. That's called explicit type casting. I will not go over type casting here.

#### Printing variables

If you need to print a variable's value, you are probably very used to being able to just include a variable name in a string. For example, in Python, you would probably do something like this

```python
a = 18
print("I am", a, "years old")
```

Well, it works differently in C. Now you need to use what's called a format specifier in your string, and declare your integer variable name elsewhere. Here's an example

```C
#include<stdio.h>

int main(void){
	int a = 18;
	printf("I am %d years old\n", a)
}
```

Here are a list of some of the format specifiers used in C

| **Format Specifier** | **Description**                                      | **Example**             | **Output**              |
| -------------------- | ---------------------------------------------------- | ----------------------- | ----------------------- |
| `%d`                 | Signed integer in decimal form                       | `printf("%d",-42)`      | -42                     |
| `%i`                 | Very similar to `%d`                                 | `printf("%i",42)`       | 42                      |
| `%u`                 | Unsigned integer                                     | `printf("%u",42)`       | 42                      |
| `%f`                 | Float/double                                         | `printf("%f",3.14159)`  | 3.14159                 |
| `%.nf`               | Float/double rounded to n digits after decimal       | `printf(".2f",3.14159)` | 3.14                    |
| `%lf`                | double. Primarily used for reading double inputs     | `printf("%lf",3.14159)` | 3.14159                 |
| `%c`                 | character                                            | `printf("%c", 'E')`     | E                       |
| `%s`                 | null terminated string                               | `printf("%s", "Hello")` | Hello                   |
| `%p`                 | Prints the address of a pointer. More on those later | `printf("%p", &var)`    | 0x7ffe2e07460c (varies) |
| `%%`                 | Prints a `%` character                               | `printf("%%")`          | %                       |

## Operations list

> This following section is a near 1 to 1 copy paste from the notes of COMP 250, as the two syntaxes are nearly identical 

Here's a list of operators C employs

**Math and assignment operators**
- `+` performs addition
- `-` performs subtraction
- `*` performs multiplication
- `/` performs division. If you do a division `/` between two integers, it will perform an integer division by default. (`5/2` returns 2). To get the decimals, you need to type cast into either a `float` or a `double`
- `%` performs modulous
- `++` increments the variable by 1. Identical to `x = x + 1`
- `--` decrements the variable by 1. Identical to `x = x - 1`
- `=` assigns a value to a variable. `+=`, `-=`, `*=`, `/=`, and `%=` all function the same way they did in Python

**Comparison & Logical operators**
- `a == b` returns `true` if `a` equals `b`
- `a != b` returns `true` if `a` is not equals to `b`
- `a > b` returns `true` if `a` is greater than `b`
- `a < b` returns `true` if `a` is less than `b`
- `a >= b` returns `true` if `a` is greater or equal to `b`
- `a <= b` returns `true` if `a` is less or equal to `b`
- `a && b` returns `true` if `a` AND `b` are `true`
- `a || b` returns `true` if `a` OR `b` are `true`
- `!(a)` returns `true` if `a` is `false` and vice versa

#### Conditionals and Loops

This is how you write an if/else chain in C

```C
#include<stdio.h>

int main(void){
	
	int yourAge = 18; 
	
	if (yourAge < 18){
		printf("You cannot enter a bar anywhere in Canada");
	}
	else if (yourAge == 18){
		printf("You can enter a bar in Alberta, Manitoba, and Quebec");
	} else {
		printf("You can enter a bar anywhere in Canada");
	}
}
		
```

> You'll notice I placed the `else if` statement on a new line, while I placed the `else` statement on the same line. Both are acceptable by conventions. It's a matter of preference

This is how you write a while loop in C

```C
#include<stdio.h>

int main(void){
	
	int i = 0;
	int result = 0;
	while (i < 50){
		result += i;
		i++;
	}
	prinf("%d", result);
	
}
```

For a more interesting one, here's how you would write a for loop in C

```C
#include<stdio.h>

int main(void){
	int result = 0;
	for (int i = 0; i  < 50; i++){
		result += 1;
	}
	prinf("%d", result);

}
```


A for loop will need three code statements inside its brackets, separated by semicolons `;` `for (statement 1; statement 2; statement 3)`

- `statement 1` is executed once before the code block executes
- `statement 2` is the condition that is checked for terminating the loop
- `statement 3` is executed every time after the code block executes

The following is another way to write a while loop. This is called a do-while loop

```C
int main(void){
	int i = 0;
	do {
		i++
	} while(i < 1000);
}
```

Finally, this is how you would write a switch statement in C. A switch statement is a simpler way to write an if/else chain

```C
int main(void){
	int day = 4;
	switch (day){
	case 1:
		printf("Monday\n");
		break;
	case 2:
		printf("Tuesday\n");
		break;
	case 3:
		printf("Wednesday\n");
		break;
	case 4:
		printf("Thursday\n");
		break;
	case 5:
		printf("Friday\n");
		break;
	case 6:
		printf("Saturday\n");
		break;
	case 7:
		printf("Sunday\n");
		break
	default:
		printf("Something went wrong\n");
		break;
	}
}
```

## Errors

An important characteristic with C is that C only has compile time errors. Things like syntax errors, undeclared variables and undefined functions are flagged quickly by the compiler, alongside a lot of other things that your compiler will warn you about. 

However, if your program is able to compile, C does not feature any runtime errors. You will not see any `NullPointerException`, or `IndexOutOfBoundError` or any of those things. C completely trusts you that your code is logically perfect.

If you've done any programming whatsoever, you'll very quickly realize that is a horrible thing. You may find undefined behavior while running your code, but will not be given any pointers as to where the error began. Your code may seem to run perfectly fine, but then break some time later. It might not even break, while still being buggy

The only time your code will give an error is if it crashes. This usually happens due to the program trying to access memory that isn't allocated to it. For example, if you cause an infinite recursion

```C
void foo(){
	foo();
}

int main(void){
	foo();
}
```

Compiling this and trying to run it will result in the following message being displayed after a bit

```
Segmentation fault (core dumped)
```

This means that your program tried to access memory that isn't allocated to it. This is the only error message you will ever see at runtime, and is frequently associated with C and C++. Good luck.

## Pointers and Arrays

> This is perhaps what C is most infamous for.



To understand pointers, we first need a basic understanding of how memory works. For now, think of memory like being a giant array of bytes. The first byte is index zero, and it keeps going up, until you reach the end of your memory.

Now with that in mind, let's rename the index of each byte of memory to the byte's *address*. An address really is just an identifier for data in memory.

A **pointer** is simply that: a variable that contains an address. The terminology goes as follows. If we have variable *a*, and we store its address in *p*, we say *p is a pointer to a*

To declare a pointer variable, we use an asterisk `*` before the identifier of the variable. 

```C
int main(void){
	int *ptr;
}
```

To get a variable's address, we can use the address-of operator `&`. To dereference a pointer and get use the value it points to, we use the dereference operator `*`.

```C
#include<stdio.h>

int main(void){
	int x = 42;
	int *ptr = &x;
	printf("%d\n", x); //prints 42 to the screen
	printf("%d\n", *ptr); //prints 42 to the screen
	*ptr = 101;
	printf("%d\n", x); //prints 101 to the screen
	printf("%d\n", *ptr); //prints 101 to the screen
}
```

> Both the operator to declare a pointer variable and to dereference it is an asterisk `*`. This may cause confusion

### Arrays

An array is a way to agglomerate many items that have the same type. To declare an array in C, we use `TYPE NAME[SIZE]`

```C
int main(void){
	int a[10]; //Array of ints of size 10
	char b[30]; //Array of chars of size 30, basically a string
}
```

C uses static arrays, meaning once you declare an array's size, you can no longer change it.

To access an element in an array, we follow the array name by an integer in square brackets, called an index. The first element starts at index 0, then index 1 and so on.

```C
#include<stdio.h>

int main(void){
	int a[5] = {4, 1, 5, 2, 3};
	printf("%d\n", a[2]); //prints out 5
}
```

> Note that C will not prevent you from accessing unintialized values. For the example above, the array `a` might take addresses 100, 104, 108, 112 and 116 in memory. If we attempt to get `a[5]`, it will get whatever existing value is stored in address 120, which we did not define ahead of time. This is undefined behavior and 

You can declare multidimensional arrays (arrays inside arrays) by writing `type name[size1][size2]`. They work the same way as the ones you've already seen

### Relationship between arrays and pointers

So how are pointers and arrays related? Well let's think of an array of ints of size 5. A 32 bit integer takes up four bytes, so it would take up bytes 100, 104, 108, 112, and 116 in memory. This means `arr[0]` has address 100, `arr[1]` is `100+4`, so address 104, and so on.

Now if we assign a pointer to the array `int *ptr = arr;`, the pointer will point to the first element of the array. Therefore, it will point to `arr[0]`. This also means `int *ptr = arr[0];` is logically equivalent to the above line 

Of course, not all types have size 4. Instead of having to memorize a bunch of sizes, we can simply use the `int sizeof(ELEMENT)` to return the size as an unsigned integer in bytes.

How are pointers relating to arrays useful? One important thing about functions in C is that you cannot return an array as a return value.  This is obviously not ideal, since sometimes, we want to do that. As a workaround, we can return a pointer to the array instead.

## C Standard libraries

The following sections will highlight useful functions from several standard libraries as well as important details. For full details, you can find them by searching *"C \[insert library name]*

### `stdio.h`

This is the standard library that handles input and output. There are three main standard streams in C.
- `stdin` or the standard input, attached to the keyboard by default. All C input functions accept input from `stdin`. In general, this is where your input is stored
- `stdout` or the standard output. All C functions can write to the standard output. By default, it's attached to the screen. In general, this is where you see appear on your terminal
- `stderr` or the error stream. All error functions will write to the error stream. You can use `gcc file.c 2> error.txt` to redirect error messages to a file.

Other streams include file streams, which some `stdio.h` functions can get.

Some of the most useful functions include

- `int getc(STREAM)`: returns the next character from a stream. or EOF(end of file)
- `int getchar(void)`: the same as writing `getc(stdin)`.
- `int putc(CHARACTER, STREAM)`: puts CHARACTER to STREAM. 
- `int putchar(CHARACTER)`:the same as writing `putc(CHARACTER, stdout)`
- `int puts(STRING)`: puts STRING to standard output
- `int fgets(BUFFER, N, STREAM)`: reads N characters from the string in STREAM, and stores them in a string with BUFFER as a pointer to the string
- `int printf(STRING, OPTIONAL_ARGS)`: takes as input a string that can contain format specifiers and prints them to the standard output.
- `int sprintf(ARRAY, STRING, OPTIONAL_ARGS)` identical to `printf`, but prints to ARRAY instead of standard output.
- `int scanf(STRING, ADDRESS1, ADDRESS2,...)` reads data from standard input and writes them to the variable addresses based on the format specified by STRING
- `int sscanf(ARRAY, STRING, ADDRESS1, ADDRESS2)` identical to `scanf`, but reads from ARRAY instead of standard input

### `stdlib.h`

This is the general utilities library. It contains a wide range of functions. Here some of the most useful ones

- `int rand(void)` returns a random integer from 0 to `RAND_MAX` (at least 32767)
- `int system(STRING)` execute `STRING` system command
- `float atof(STRING)` converts `STRING` into a float
- `int atoi(STRING)` converts STRING to an integer
- `int abs(INT)` returns the absolute value of INT
- `void exit(INT)` terminates calling process with exit code `INT`'
- `malloc(), calloc(), realloc(), free()`. Related to dynamic memory management. See later.

### `math.h`

This library has a lot of functions related to math. Most of these are self explanatory, so I won't reexplain them here. Just look at [the following list ](https://cplusplus.com/reference/cmath/) to find the math function you need

> Interestingly, there is no pi or euler's number constant stored anywhere in the math module. If you need those values, you can set a constant variable to both those values. 

### `string.h`

This library has functions that allow operations on strings. Here are some of the most important functions

- `int strcmp(char *str1, char *str2)` compares two strings. Returns zero if equal and a non zero integer if not equal
- `int strncmp(char *str1, char *str2, size_t num)` Same as above, but you can specify how many characters to compare
- `size_t strlen(char *string)` returns the length of a string
- `char *strcpy(char *dest, char *src)` Copies string from src to dest.
- `char *strncpy(char *dest, char *src, size_t n)` same as above but for n characters
- `char *strcat(char *dest, char *src)` Appends src to dest
- `char *strncat(char *dest, char *src)` same as above but for n characters
- `void memset(void *ptr, int value, size_t n)` Set the first n bytes of ptr to value.

> `size_t` is an alias for an unsigned integer. You can use `int` instead. It will be implicitly casted.

## Header files

Something I like to do is read source codes on GitHub. I don't know, I find it fascinating. Once the source code for the [stockfish chess bot](https://github.com/official-stockfish/Stockfish/tree/master/src), written in C++. For every file, I noticed there's a file ending in `.cpp`, which is the C++ source file. However, I also noticed a file ending in `.h`. At the time, I was stumped on what that could possibly be

Although the above example is in C++, the same principle applies in C. The `.h` files in question is called a header file. Its purpose is to include a bunch of function declarations. But why do we use them?

Unlike most other programming languages, there are more than one compiler that we can use to compile a C program. We can categorize those into two categories

- Single-pass compilers: These compilers will go through your code exactly once, from top to bottom, in order to compile the file
- Multi-pass compilers. These compilers will go through your code multiple times to compile the file.

Although most modern compilers are single pass, including gcc, if you are to write a C file, you would like your code to be able to work regardless of the compiler used. This means you have to take into account the single pass compiler.

Why is this important? Let's take a look at the following C file

```C
#include<stdio.h>

int main(void){
	int x = 10;
	int y = 20;
	int result = add(x,y);
	printf("%d\n", result);
	return 0;
}

int add(int a, int b){
	return a + b;
}
```

You'll notice we're making a function call to `add()` before we properly declared or defined it. This is not an issue for multi-pass compilers, but for a single pass compiler, the compiler won't know what to do when `add()` is called in `main()`. When that happens, there are two potential outcomes. You program might fail to compile and return a compile time error, or the compiler might substitute your function with a function that takes the inputs and returns and returns an `int` of zero, which is incredibly unsafe and can cause a whole lot of undefined behavior.

This leads back to the header file. The purpose of a header file is to include a bunch of function declarations so that the compiler can see. In essence, they're a bunch of prototypes telling the compiler "Hey, we are actually functions in this program. I promise we'll be defined soon"

In the previous example, our header file (call it `demo.h`) would have only one line

```C
int add(int a, int b); // Notice the semicolon
```

And now, we can write an include statement at the top to connect our header file to our source code file

```C
#include<stdio.h>
#include"demo.h"

int main(void){
	int x = 10;
	int y = 20;
	int result = add(x,y);
	printf("%d\n", result);
	return 0;
}

int add(int a, int b){
	return a + b;
}
```

Of course, this example isn't particularly helpful, but it will become more useful once we dive into modular programming

> For similar reasons, your main function should always be at the bottom of your file. If you're used to putting your main functions at the top in Java, you should change that habit.

## Dynamic memory allocation

> All sections below are poorly written, I might rewrite it one day, maybe

A feature in most modern high level programming languages is automatic memory management. The compiler/interpreter will do the memory management for you so you can worry more about the logic that runs your code. While that's easier on the programmer, they can be slow. 

In C, the control over memory is handed over to the programmer (you). This is the basis of dynamic memory management

Let's look again at four `stdlib.h` functions we glossed over earlier

- `void *malloc(size_t size)` allocates `size` bytes of memory to a pointer and returns it
- `void *calloc(size_t n, size_t size)` allocates memory for `n` elements of size `size` to a pointer and returns it
- `void *realloc(void *ptr, size_t size)` attempts to resize `ptr` to `size` bytes of memory
- `void free(void *ptr)` deallocates any memory that was allocated to `ptr`.

This allows more control for the programmer, but can lead to serious bugs if done improperly

## Structs and Unions

C does not support OOP. However, we can create structures, called `struct`, which group several related variables in place.

Here's how you would create a struct

```C
struct Student{ //Declare struct
	char name[30]; //These fields are called members
	int age;
	float gpa;
}; // End declaration with semicolon
```

Here's how you would create a new struct, and access its members

```c
#include<stdio.h>

int main(void){
	//Declare a new struct
	struct Student s1;
	
	//Assign values using the dot notation
	s1.name = "Alice";
	s1.age = 20;
	s1.gpa = 3.9;
	
	//Print values
	printf("Student %s is %d years old and has a %.2f gpa\n", s1.name, s1.age, s1.gpa);
	
	return 0;
}
```

Typing `struct NAME_OF_STRUCT nameOfVariable` is very tedious though. To simplify a struct that is used often, you can use `typedef`, which will let you generate a new name for your data types

```C
typedef struct{
	char name[30];
	int age;
	float gpa;
} Student; // Here, student is the name of the new type.

int main(void){
	
	Student s1; //Notice how we didn't use the word struct
	
	s1.name = "Alice";
	s1.age = 20;
	s1.gpa = 3.9;
	
	printf("Student %s is %d years old and has a %.2f gpa\n", s1.name, s1.age, s1.gpa);
	
	return 0;
}
```

> Just like any other type, you can declare arrays of structs

### Unions

Unions are very similar to structs, but only one object is stored in memory at a time. 

```C
#include<stdio.h>

union Student{
	char name[30];
	int age;
	float salary;
};

int main(void){
	union Student s1
	
	s1.name = "Alice" //Only this value will be stored in memory
	printf("%s\n", s1.name); //This is fine
	s1.age = 21; //This would replace the name field
	printf("%s\n", s1.name); //This would now lead to undefined behavior
	
	return 0;
}
```

Unions are particularly useful when used in combination with structs. You can use them to create a general data type. Here's an example with banking accounts

```C
struct account{
	float balance
	char[50] ownerName;
	union{
		struct checking{
			float overdraftLimit;
		};
		struct savings{
			float interestRate;
		};
		struct investment{
			float averageReturnRate;
			int riskLevel;
		}
	}
}
```

This is probably not the best example, but you get the idea.

## Files

Will write later

# Unit 4: Developer Tools & Techniques

[^ Back to top](#table-of-contents)

This unit will overview a lot of the tools and techniques used by developers to work efficiently

## Writing code in a team environment

There are several elements that go into writing code efficiently in a team setting without your entire team hating you. This includes code readability, sharing code, working well and using tools to help management.

For most languages, there is a set of syntax standards to use to write code that can be read by anyone. If you're working on a massive project though, you would probably want to take organization further. In an industry, a style might be imposed. For now, here's a snippet of code that could work well in a team environment

```c
// Programmer: Alice Doe
// Created: 2025/08/11
// Last Modified: 2025/08/11
// Purpose: Returns the amount of times an element appears in an array

int elementCount(int arr[], int e, int length){
	int i     = 0;
	int count = 0;
	
	//counter loop
	for (i = 0; i < length; i++){
		if (array[i] == key){
			count++;
		}
	}
	
	return count;
}

```

> Notice the paragraphs, alignment, comments and more

On top of code format, folder format is preferably consistent as well. This also comes down to the industry and the team, but in general, there are a few recurring directories

- `docs` a directory that stores documentation about the code
- `backups` a directory containing stable releases of the project
- `releases` a directory containing all versions of your program good for release
- `assets` all non-code and non-data files your project uses (images, videos, audio)
- `database` all database files (.csv, sql)
- `src` all code files

### Team management

Of course, there are more that goes to a good team workflow. The concepts here don't just apply to computer science.

To manage time, it is best to set deadlines to team members to ensure the workflow is in order. A table helps with that tremendously

A project fails without organization, equal hard work from everyone, and communication

Specific to computer science though, you want a good source code management tool to keep track of changes in a codebase. There are many options, but by far the most popular option is Git.

## Modular Programming

Modular programming is a way of decomposing a program into multiple objects. The idea goes as follows: we create our program using many `.c` files, compile each source as an object, and merge them together.

To help with that, we use header files `.h` to "promise" our compiler that our function exists elsewhere and to help with a bunch of things.

To compile into an object file, we can use the `-c` flag on the compiler. This will return a `.o` object file, that we can keep and later combine easily with other `.o` files to compile our final program.

### Pre-processor macros

Before your source code is compiled, the compiler might make changes to it. To do so, it uses pre-processor commands.

We have already seen one of them, being `#include`. This imports all of the indicated code into your file before compilation. Others that will be seen are `#define`, `#ifdef`, and `#ifndef`

`#define` will define new terms not part of the C language. It's essentially indicating a way to subsitute a word with another. For example, we can "add booleans" with this

```C
#define TRUE 1
#define FALSE 0
```

`#ifdef` and `#ifndef` work similarly. If a term has been defined by `#define`, it will return true for `#ifdef TERM` and execute the code within its block. `#ifndef` will execute the code if it has not been defined. In both macros, the code block is terminated by `#endif`.

### Makefile

A makefile is a way to help with modular programming. Assume you have a program comprising of 100 `.c` files.  If you want to compile that with `gcc`, you would have to do `gcc file1 file2 file3 ...` for every time you want to compile the file for any reason, which is incredibly tedious. To help, you can create a `Makefile`

A basic makefile has the following syntax

```
label: used_labels
	command
```

You assign a `label` to each command line, and `used_label` to indicate which other labels that label uses. Afterwards, you execute the commands. Here's a sample makefile

```
program: file1.o file2.o
	gcc -o program file1.o file2.o
file1.o: file1.c
	gcc -c file1.c
file2.o:
	gcc -c file2.c
backup:
	cp *.c ./backup
```

Now to run the makefile in your project, you use the command `make`. This will make it execute the top label in the makefile. You can also add a label `make LABEL` to specify which label to execute.
## Git & Version Control

Version control refers to the practice of controlling organizing and tracking different versions in history of code files. We call a database containing all versions of a file system a **repository**. There are a couple choices, but the most popular one by far is git.

Git is a distributed control system that is widely used by developers across the world. It is primarily a command line software, although applications exist that give a GUI to the process.

To get started, you can `git init` a new repository. To work on an existing repository instead, you can `git clone [url]`.

Doing either should get you the most up to date version of your repository. When you finish making your changes, you can `git add .` to add all files to a change batch (called commit), and `git commit -m "meaningful message"` to commit your messages to your local repository. To sync up with the repository everyone has access, you need to `git push` so that your changes appear on the main repository.


You probably don't want to do this on the `main` or `master` branches though. A **branch** is simply a deviation from a code typically used by developers to test new code without breaking the old functional one. You can `git branch` to list a list of branches, and `git branch [name]` to create a new branch or `git checkout [branch]` to move to an existing one. After you're done making modifications to a branch and confirmed they aren't broken, you can go back to main/master branch and `git merge [name]` to merge your branch to the main/master branch

Of course, you are probably not the only one working in a repository. Thus, you should `git fetch` to fetch all changes made since your current version, and `git merge` to merge the remote branches with your current branch. Alternatively you can do `git pull`, which does both.

Finally, use `git log` to see a log of all commits, and `git status` to see the status of your current unstaged commit.

[Git Cheat Sheet by GitHub Education](https://education.github.com/git-cheat-sheet-education.pdf)

## GNU tools & other developer tools

This part will briefly overview a bunch of tools from the GNU project and others
### gprof

`gprof` is a profiling tool in GNU. Profiling is like a stopwatch for each part of your program, and can be used to spot inefficiencies.

To use gprof, compile your C file with the `-pg` library needed to make gprof work. Then, run your program. Finally, use `gprof your_program` to see a report of how much resources each function is using

### gdb

gdb is a debugger for C. It has many features useful for debugging, such as setting up breakpoints, checking a variable's value, modifying a variable's value, and more

To get started, compile your C program, with the `-g` flag to set up things needed for gdb

Then run `gdb your_executable`. You will enter the gdb menu. Search online for a guide of all gdb commands to use. I recommend [this one](https://www.geeksforgeeks.org/c/gdb-step-by-step-introduction/)

### valgrind

This tool is not part of the GNU library. The format is `valgrind COMMAND_FOR_YOUR_EXECUTABLE`. What this program does is analyze your memory allocation, detect memory leaks, and unallocated memory. This is incredibly useful in C

# Unit 5: Systems Programming

[^ Back to top](#table-of-contents)

> This unit will overview systems programming and introduce it very briefly. It will be explored a lot more in future courses

**Systems programming** involves coding systems that interact with the hardware or operating system directly. Coding operating systems is part of this category.

C is a popular choice for systems programming due to its low level capabilities as a high-level language (and its performance). As such, it allows direct access to a computer's devices. 

## Manipulating binary in C

As machines can only understand binary, C offers several operators to manipulate binary

- `&` the binary AND operator. Ex: `1011 & 0110 == 0010` 
- `|` the binary OR operator. Ex: `1011 | 0010 == 1111`
- `~` the binary NOT operator. Ex: `~1011 == 0100`
- `>>` the binary shift right operator. Ex: `01011010 >>3 == 00001011`
- `<<` the binary shift left operator. Ex:`01011010 <<3 == 11010000`

## Processes

Processes are programs currently active in memory. There are three types of processes
- Shell processes are processes that run in their own shell. When the program is called, it will launch a new shell to run the program (ex `stdlib.h`)
- Clone processes are processes which can clone itself. (ex `fork()`)
- New processes are processes which can launch a new program within the current shell.

## `system()` and `fork()`

`system()` is a `stdlib.h` function that allows C to execute any program within the shell its in. For example, you can run `system("ls")` to make C run `ls` in bash

`fork()` is a `unistd.h` function which duplicates the calling process. It's a bit complicated to explain, so this function is left as exercise to the reader