# Lab Report 1 - Remote Access and File System

I'm pretty familiar with the linux filesystem overall, so much of this was a review for me. However, it's still nice to get a refresher. Note that all file paths below will be relative, typically to `/home`.

### cd command
```bash
[user@sahara ~/lecture1]$ cd
[user@sahara ~]$ 
```
We were originally in the `~/lecture1` folder, but after running `cd` we return to the home folder. Since it has no arguments, `cd` will default to returning to the home folder.

This output is NOT an error

```bash
[user@sahara ~]$ cd lecture1/
[user@sahara ~/lecture1]$ 
```

This time, we were in the `home` directory, and used `cd` with the argument of `lecture1/` to move into the lecture1 folder. 

This output is NOT an error

```bash
[user@sahara ~/lecture1]$ cd README 
bash: cd: README: Not a directory
```
While in `lecture1/` again, we try to `cd` the file `README`. However, since this is a file and not a directory, cd errors. We cannot "move into" a file.

This output IS an error.

### ls command
```bash
[user@sahara ~]$ ls
lecture1
```

In this instance, we are in the home folder. Using `ls`, we list the contents of the directory. In this case, we see that the folder `lecture1/` folder is in the home directory.

This output is NOT an error.

```bash
[user@sahara ~]$ ls lecture1/
Hello.class  Hello.java  messages  README
```

Now, we use the `ls` command (while still in the home folder) with the argument of `lecture/`, which gives us the contents in the `lecture/` folder.

This output is NOT an error.

```bash
[user@sahara ~/lecture1]$ ls Hello.java 
Hello.java
```

Now in the `lecture/` folder, running the `ls` command on the *file* `Hello.java` we get an output of name of the file argument that we have put in. This isn't an error - `ls` will list file contents and/or any associated information (given by flags on `ls`). For example, if we run 
```bash
[user@sahara ~/lecture1]$ ls -l Hello.java 
-rw-r--r-- 1 user user 332 Oct  5 08:53 Hello.java
```
we can see that we get back some more information.

This output is NOT an error.


### cat command
```bash
[user@sahara ~/lecture1]$ cat
^C
```
When running the `cat` command in the `lecture/` folder with no argument, it will continue to wait for an argument to be entered. I had to escape this using the `ctrl + c` sequence. This isn't explicitly an "error" per se, but certainly we aren't going to get out any meaningful information without a file argument.

This output is KIND OF an error?


```bash
[user@sahara ~/lecture1]$ cat messages/
cat: messages/: Is a directory
```
Running the `cat` command in `lecture/` on the `messages/` directory gives us an error because `messages/` is a directory, not a file. `cat` is expecting a file as an argument.

This output IS an error.


```bash
[user@sahara ~/lecture1]$ cat README 
To use this program:

javac Hello.java
java Hello messages/en-us.txt
```
In the `lecture/` folder, we run `cat` on the README file. We get out the entire contents of the README file, as per what the `cat` command is supposed to do.

This output is NOT an error.

