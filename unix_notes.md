# Lab 1: Just Enough Unix for Bioinformatics

* Get to the terminal by accessing it through VCN
* Format: COMMAND OPTIONS FILEPATH
* Manual Pages: man ---> man date
\

**1. Exercise One: Print out the unabbreviated day of the week**
```
# Code Block for Unabbreviated Day of the Week:
date "+%A"
```
* Tab Completion --> Finishes word for you when pressing tab. Can press multiple times. Should use for file names
* Won't use environment variables as much but can be important for some programs for configuration.
\

### Defining User Variables
```
# Example Setup for User Variables
mycolor="blue"
```
Tell the shell you are referring to variable by placing a "$" in front of it.  

```
# Recalling User Variable
echo $mycolor
```  

**2. What happens if you type `echo mycolor`? Explain what you get and what echo does and why does it behavior change with the "$"**  

* Echo simply repeats whatever word that follows it. It seems that the "$" sign is able to actively recall the saved variable from before.  

### Files & Directories
File system is where your files are stored. To see how much free space you have on system, use:
```
df
df -h
# -h makes it more human friendly
```

Another command shows us how much space each of your files and directories uses:
```
du -h
```  

**3. Your instance's main file system is located at `/dev/sda1`. Using what you know about Unix commands and the `df` function, print out how much free space your instance has available in a human-readable format.**
```
# We want to find the amount of disk space is available in our main file system located at `/dev/sda1` in a human readable format.  

df -h /dev/sda1  

# This gives us: Filesystem      Size  Used Avail Use% Mounted on
                /dev/sda1        78G   20G   59G  25% /
# We now know we have 59G available on our `/dev/sda1` system
```  

### Creating and Viewing Files
The `touch` command will create a file or change its modification time
```
touch foo
``` 

The file `foo` doesn't contain anything. To create a file with some content in it
```
cat > bar
```

  * When hitting enter, you can enter any amount of lines. To end the file, hit `CTRL-D`. 
  * To see contents of the file, use `cat` command to view. 
  * You can inspect first 10 lines of file with `head` command.
  * You can inspect last 10 lines with `tail` command.
  * Both commands have options to see different number of lines, for example `head -1 bar`, to see just the first line
    
    ```
    cat bar
    head bar
    tail bar
    head -1 bar
    ```

### Editing Files  

You can edit files with nano or Atom. Staying in the terminal, use `nano`
```
nano bar
```  

You will be prompted to the editing page where you can add/delete lines. Focusing on the commands at the bottom will help facilitate what you want to do. To save the changes you have made, hit the `^O` key which is control + letter o. To exit use `^X`.  

Unix file names often have the following properties:  

  * all lowercase letters
  * no spaces in the name (use underscores or dashes)
  * an extension such as .txt or .md  
    
### Navigating Directories  

To find out what path you are currently in, use `pwd` (print working directory)  

```
pwd
```

This tells your current directory.  

To see what files are in your current directory, you can use `ls`
To see the type of files in your directory, you can use `ls -F`, with the forward slash indicating the file is another directory (either below or above you)  

   * There is at most one directory above you which is the **parent directory**. To find how what your parent directory is, use: `ls ..` 
   * To find all options for ls, you can use `man ls`  
   
There are 2 ways to specify a directory: **relative** & **absolute** path.  
The above example commands were used to find the relative path. If you would like to edit a file elsewhere that you are not already in, you could specify the absolute path by using a forward slash `/`  

To list the (1) absolute root of the Unix file system & (2) the contents of `/usr/bin` you would use:
```
ls /
ls /usr/bin
```  

`ls /usr/bin` will show you all contents in your current folder while `ls ..` shows what is in the parent directory.  

To change your working directory, you can use `cd` (change directory).  

To go back to the root directory, we could use the variable `HOME` to find the location of your home directory. The tilde key, `~`, is another shortcut for the same command. If you want to use the `HOME` variable, you have to start it with a `$`.
```
cd /
touch $HOME/file1
touch ~/file2
# Both ways to add files
```  

```
# Creating files using relative & absolute paths instead of short cuts  

touch /home/exouser/file3     # Adding file 3
cd ~/Documents      # Change Directory to documents
pwd     # Where you are
ls      # List contents
touch ../file4      # Add file 4 to parent directory
```  

### Moving and Renaming Files  

To create a new directory, use the `mkdir` command (make directory)
```
cd
mkdir Stuff     # Good practice to capitalize directories 
```  

To move files into that directory, use `mv` (move) command
```
mv foo Stuff
```  

If there are 2 arguments and the 2nd argument is not a directory, `mv` will rename the file.  

You can move multiple files at one time. The last argument is where you want to move it to
```
mv file1 file2 file3 file4 Stuff
ls
ls Stuff
```  

### Copying & Aliasing  

The `cp` (copy) command copies files.  

When editing copied files, that version will change compared to the file it was copied from. 
```
nano barf
# Add edits
cat barf
cat Stuff/barf
```  

When copying, you can change the name from the original file.
```
cp Stuff/barf ./bark
ls
```  

The `*` command in a wildcard which will fill in missing letters or arguments. Every file starting with `ba` will be moved to stuff.
```
mv ba* Stuff
ls
```  

An alias is another name for a file. The link command, `ln -s` is useful to organize files/directories.  

  * Since we linked the Stuff and foo together using `ln -s` when we edit one linked file we edit the other one.
  * To see that foof is an alias for Stuff/foo using the `ls` command with the `-l` argument
  ```
  ls -l foof  # Accidently make folder name food, not sure if it is linked
  ```  
  
### Deleting File  

You can delete files with `rm` (remove). **Once they are deleted they are gone forever.**
```
ls Stuff
rm Stuff/file1
ls Stuff
rm foof
```  

  * Don't forget about tab to save time  
  
### Using File Permissions  

A file can have 3 kind of permissions:  

  1. Read
  2. Write
  3. Execute  
  
This is abbreviated as `rwx` when you do `ls -l` Programs & directories need executable permission to access them. Important data files should not be edited and to ensure that, they should have read only permission.  

```
touch foo.txt
ls -lF foo.txt
```  

This produces:
```
-rw-rw-r-- 1 exouser exouser 0 Apr  6 14:15 foo.txt
```  

Shows permissions, # of links (1), owner (exouser), group (exouser), file size (0 bytes), date, and file name.  

To turn on all permissions for everyone:
```
chmod 777 foo.txt
```  

Produces: 
```
-rwxrwxrwx 1 exouser exouser 0 Apr  6 14:15 foo.txt*
# Hyphen as first character indicates ordinary file while a d will be a directory
# Table in notes to reference
```  

To change the permission of a file, use `chmod` (change mod). 2 different ways but human friendly one is:
```
chmod u-x foo.txt
# Change the user (u) to remove (-) the execute (x) permission from file foo.txt
# Add permission with +
```  

Octal format is the different syntax.  

  * 4 is the read permission
  * 2 is the write permission
  * 1 is the execute permission
  * Each rwx corresponds to one octal number from 0 to 7
  * `chmod 444` anybody can read, nobody can write
  * `chmod 644` I can read/write, group/public can read  
  
### Working with Text Files  

Two types of text files: text & binary  
  * Text is normal format we're use to
  * Binary is a bunch of random letters  
  
Text files have different formats depending on system. Avoid interchanging files between Linux, Mac, and Windows.  

**"One of the stupidest things you can do is to put sequence data into Microsoft Word or Excel (or similar software) and then attempt to use it in Unix."**  

You might want to open file sequences in text editor but you shouldn't:  

  1. You shouldn't edit data files
  2. It takes more memory to edit than view
  3. The file is binary (gibberish)  
  
```
ln -s ~/data/C.elegans/c_elegans.PRJNA13758.WS269.genomic.fa.gz ./genome.gz
ls -lF
```  

`ln -s`: creates an alias from this directory to the original file.
`ls -lF`: The l at the beginning of the permission shows that the file is an alias. Arrow shows you what the alias points to.  

To start uncompressing it, use `gunzip` (GNU unzip, don't anger the CS mob and say gunzip). To show the file in the terminal that is similar to `cat`, use `gunzip -c`
```
gunzip -c genome.gz
```  
The contents should show on your terminal. To stop is, use `^C (control-C)`or `^Z` which sleeps the process.  

To look at the first few lines: `head genome.gz`  

Unix can pipeline the output of 1 program into another using the pipe `|` token. This helps when the format is in binary but you want a readable version. 
```
gunzip -c genome.gz | head
```  

`less` allows you to search for simple patterns. Hit the forward slash `/` to bring up the search prompt at the bottom of the terminal. Now hit the `>` key and press return. `less` will find the next `>` symbol in the file. Keep hitting the `/` key and return. `less` remembers the last pattern.  

The Unix command `grep` is often the first tool people use to find patterns in files.
```
gunzip -c genome.gz | grep ">"
# This produces  

>I
>II
>III
>IV
>V
>X
>MtDNA
```  

To save that output to a file, you can redirect the output to a file with the `>` token.  

```
gunzip -c genome.gz | grep ">" > chromosomes.txt
cat chromosomes.txt
```  

**4. The file ~/data/A.thaliana/Araport11_genes.201606.pep.fasta.gz` contains all of the predicted protein sequences from genes in the Arabidopsis genome. Using the commands you learned so far, write a command or set of commands to dispay the name of the last protein in file. In your answer include both the commands that you need (formatted as a code block) and the output from those commands.**
```
> ln -s ~/data/A.thaliana/Araport11_genes.201606.pep.fasta.gz ./protein.gz
> gunzip -c protein.gz | tail
> gunzip -c protein.gz | grep ">"
> gunzip -c protein.gz | grep ">" > A_thaliana_proteins.txt
> cat A_thaliana_proteins.txt
```
The last protein in the file should be:
```
>ATMG01410.1 | open reading frame 204 | ChrM:366086-366700 REVERSE LENGTH=204 | 201606
```  

To reduce file size, you can use `gzip` (GNU zip) and `gunzip` (GNU unzip) as needed. `gzip` usually reduces a file to between 50% and 25% its size, so is useful to save space, especially with large -omic data files.
```
gzip chromosomes.txt
ls -l
gunzip chromosomes.txt
ls -l
```  

If you would like to compress/decompress a file, but keep the original file you can use the `-k` argument for example: `gzip -k chromosomes.txt` 
```
gzip -k chromosomes.txt
ls -l
```  

We should have a `chromosomes.txt.gz` file and the original `chromosomes.txt` file.  

When people send you multiple files, they are generally sent as **tar ball**. The `tar` (tape archive) command creates archives, which are single files that contain multiple files. Example:
```
touch file1 file2 file3 file4 file5
```
```
cd ..
ls
ls Project0
```  

To create an archive of Project0, we need to tell the `tar` command that we want to create the archive with the `-c` option and tell if the name of the file with the `-f` option
```
tar -c -f project0.tar Project0
# Can combine single letters to form: `tar -cf project0.tar Project0`
```  

Unix options generally go before the arguments on command line. Thats why `-f project0.tar` goes before `Project 0`  

Compres tar-balls
```
gzip project0.tar
ls
```  

The `tar` command will do this automatically with the `-z` option. As a one-liner looks as follows:
```
tar -cf project0.tar Project0
```  

To decompress a tar-ball, you swap the `-c` for the `-x` option. 
```
mv project0.tar.gz Stuff
cd Stuff
tar -xzf project0.tar.gz
ls
ls Project0
```

# stopping at processes because im losing my mind

  

