# Level 0 --> 1
Login with 
$ ssh bandit0@bandit.labs.overthewire.org -p 2220
	//the username is bandit0
	pw is bandit0. pw's do not move the cursor or show any characters SO BE EXTRA CAREFUL.
	-p 2220 is the cmd to connect through port 2220 as per directed through overthewire.
	this one line connects as username bandit0 to the website after the @ sign//

cmds learned
	ssh  Secured Shell
	-p  port
    * gef (https://github.com/hugsy/gef) in /opt/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /opt/pwndbg/
    * peda (https://github.com/longld/peda.git) in /opt/peda/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /opt/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)


cmds learned
	ls  lists directory comments		
		https://man7.org/linux/man-pages/man1/ls.1.html

	cat  prints files to standard output
		https://man7.org/linux/man-pages/man1/cat.1.html

	man (then list whatever cmd to learn more on what flags go with what)

	file  determine file type
		https://man7.org/linux/man-pages/man1/file.1.html

	du  estimate file usage
		https://man7.org/linux/man-pages/man1/du.1.html

	find  finds the file by name. doesn't open it on default tho
		https://man7.org/linux/man-pages/man1/find.1.html

	cd  change working directory
		https://man7.org/linux/man-pages/man1/cd.1p.html


# Level 1 --> 2
Opening dashed (hidden) files
cat acts likes a basic command for standard inputs and expects something after - (i think???)
	cat -  brings up nothing. However
	cat ./-  specifes that there is a file called "-" to open up
	cat < -  works too

https://www.golinuxcloud.com/overview-bash-dashed-filename-directory-linux/#What_is_single_dash_-_and_double_dash_%E2%80%93_in_Posix
https://tldp.org/LDP/abs/html/special-chars.html


# Level 2 --> 3
the filename was "Spaces in this filename" that had the key.
must use ' ' to find the file.
Using
cat spaces in this filename  checks for files named spaces, in, this, filename instead.
ctrl+d escapes duplicate cat entries


# Level 3 --> 4 
directories are folders. inhere folder had a file '.hidden'. The dot makes it hidden. 
cat./ the file and bam


# Level 4 --> 5
ls -la to list everything. Shows that every file starts with '-file00, ...01, ...02'. 
	file ./-file0* will show characteristics of every file with '-file0' in the name. All files were just data except one. -	file07. ]
	cat ./-file07
A faster way
	If we want to apply/use a command on all the files in the current directory, repeating the command or writing all filenames is tedious. To use the command on all the files in the directory without a lot of writing, we can use ‘*’, which is called a ‘wildcard symbol’. ‘*’ can stand for any number of any literal characters. An example could be file*, which would match to everything that starts with ‘file’, like ‘file00’, ‘file’, ‘fileAA’ and so on. It replaces the filename/path option in the command.

The most common data encodings that are human-readable are ASCII and Unicode.

# Level 5 --> 6

https://man7.org/linux/man-pages/man1/du.1.html
https://man7.org/linux/man-pages/man1/find.1.html

ls gives a list of 20 folders with many files inside each. The file I'm looking for is exactly 1033 bytes. 
    find -size 1033c returns ./maybehere07/.file2
	cat ././maybehere07/.file2 and bam

I have already given an introduction to the file command and how I used it to detect human-readable files in Level 5. The file command alone worked well for a small number of files. However, with more files, it is easy to lose the overview.

The command grep searches its input for lines containing a specific pattern defined by the user. It can also be used to do the opposite, meaning when using the -v flag, a line with a defined pattern will not be printed.

To use this command on the output of another command (for example, the file command), we use the pipe |. It takes the output of the first command and pipes it as input into the second command. The syntax could look something like this: <command1> | grep <pattern>.

To get the file size, we use the du command. Specifically, to get the size in bytes, we also use the -b flag. To look at all the files, including hidden ones, the -a flag is offered.

Addition: The ls -l command also shows the size of files in the fifth column.


Side note**The command file */{.,}* would return the file type of every file in the folders in ‘inhere’. We use */* to print all files in all directories. However, this does not include hidden files. Therefore we use {.,} to include files starting with a . and , indicates files starting with anything else



# Level 6 --> 7
A tough one. I had to read through each file. Thankfully there's only one that would allow permission to read. 
A ls -a reveals that there are three folders. I'm looking for the user bandit7, group bandit6, and the file size is 33 bytes. 
	find / -user bandit7 -group bandit6 -size 33c
		the '/' makes the machine start looking from root first.
After finding the single file that I had permission to read, a simple cat revealed the key. 

2>/dev/null
	removes any lines with permission denied 


# Level 7 --> 8
cat data.txt but you need to use | with grep. Grep shows you the line the word is found in
	cat data.txt | grep millionth
		millionth is beside the key

# Level 8 --> 9

You can sort files. for level 8, sort data.txt with | uniq -u
	so sort data.txt | uniq -u


# level 9 --> 10

??? kinda easy
use 
	strings data.txt
		printed out a bunch of lines that were readable. The password stuckout like a sore thumb


Level 10 --> 11
       base64 - base64 encode/decode data and print to standard output

SYNOPSIS
       base64 [OPTION]... [FILE]

DESCRIPTION
       Base64 encode or decode FILE, or standard input, to standard output.

       With no FILE, or when FILE is -, read standard input.

       Mandatory  arguments  to  long  options are mandatory for short options
       too.

       -d, --decode
              decode data

       -i, --ignore-garbage
              when decoding, ignore non-alphabet characters

       -w, --wrap=COLS


password is hiding in data.txt file and is encoded with base64
	-d decodes so..
		base64 -d data.txt


echo 'ABCdefGHIjkl' | tr '[a-z], [A-Z]' [n-za-m], [N-ZA-M]'
**Learned later that this is ROT13, a cmd that cygwin does not have installed. I later used a Kali VM and downloaded thousands of cmds for convinience/practicallity.

# Level 11 --> 12

transforming is wild
	to transformm a letter or set, just do ..... | tr e o
		so every "e" is replaced with “o” in the output.
		now for rotations, see below. It’ll remind you. simple, but easy. think matrixes when you’re doing both lower case and upper cases that need to keep their cases but still tranform/rotate

| tr '[a-z],[A-Z]' '[n-za-m],[N-ZA-M]'
		cat data.txt | tr '[a-z],[A-Z]' '[n-za-m],[N-ZA-M]'
	




