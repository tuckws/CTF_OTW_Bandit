# CTF_OTW_Bandit

## Purpose
OverTheWire provides excellent Capture-the-Flag (CTF) exercises in various cyber domains of knowledge. The Bandit Series is focused on developing knowledge of Linux in order to complete the challenges outlined below.

## Skills Learned
-Linux commands, file operations, navigation, cron functions, and permissions management.
-Combining and piping functions in the command line. 
-Linux networking to include use of `nmap`, `netcat`, and `ssh`.
-Git functions in Linux. 

## Steps

**SPOILERS BELOW**

The keys listed are for access to the under which they are listed.
SSH Information  
	Host: bandit.labs.overthewire.org  
	Port: 2220
	`ssh user@host -p <port>`
My personal directory on bandit.labs.overthewire.org: `/tmp/tuktuk92`
##### bandit0
bandit0
##### bandit0 to 1
>The password for the next level is stored in a file called **readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
##### bandit1 to 2
>The password for the next level is stored in a file called **-** located in the home directory

Dash filename. 
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi
##### bandit2 to 3
>The password for the next level is stored in a file called **spaces in this filename** located in the home directory

Spaces in filename (tab or \ like programming) and/or quotes. 
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG
##### bandit3 to 4
>The password for the next level is stored in a hidden file in the **inhere** directory.

Hidden file. Same as previous level with quotes. 
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe
##### bandit4 to 5
>The password for the next level is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the “reset” command.

Used `file` to identify which file in the list had the readable text, annotated with ASCII or Unicode. 
lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR
##### bandit5 to 6
>The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:
- human-readable
- 1033 bytes in size
- not executable

Used `find ./ ! -executable -size 1033c -exec cat {} +` You cannot pipe commands into `ls` because ls uses command input not standard input or output. You can use `-exec` in order to execute commands similar to | . 
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
##### bandit6 to 7
>The password for the next level is stored **somewhere on the server** and has all of the following properties:
- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

Used `find / -user bandit7 -group bandit6 -size 33c 2>/dev/null -exec cat {} +` Again, using `-exec` to "pipe" commands with `cat` (`ls` is this way too). 
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
##### bandit7 to 8
>The password for the next level is stored in the file **data.txt** next to the word **millionth**

Used `grep "millionth" data.txt`
TESKZC0XvTetK0S9xNwm25STk5iWrBvP
##### bandit8 to 9
>The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

Used `cat data.txt | sort | uniq -c` Then, I identified the only line repeated once by count. 
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
##### bandit9 to 10
>The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

Used `strings data.txt` This stripped out non-ASCII characters, and I identified the passcode. 
G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
##### bandit10 to 11
>The password for the next level is stored in the file **data.txt**, which contains base64 encoded data

Used `base64 -d data.txt` This decoded the base64 text. 
6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
##### bandit11 to 12
>The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

Googled ROT13 decryption. Used `cat data.txt | tr 'a-zA-Z' 'n-za-mN-ZA-N'` Remember for command piping that the output is going into the following command. This is why you would need to use `cat` or `echo` in order to generate text with which to use for the next command. 
JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
##### bandit12 to 13
>The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

This one really stumped me, and I referenced https://david-varghese.medium.com/overthewire-bandit-level-12-level-13-2ec761a88907 at first. Once I understood the mechanics of compression I quickly figured this out. I was first confused that hexadecimal needed to be converted to binary in order for linux to use `file` on it and see what the compression type was. The `hexdump` on the file prevented running `file` on it. 
I learned that you can technically compress a file in different ways sequentially. When you `file` the file after a decompression, you can see if it is still compressed or if it is uncompressed into the "original" ASCII form. This was a really interesting exercise that helped me better understand file compression and some linux commands to include `xxd` `gzip` `bzip2` `tar`. The man pages are really useful!
wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
##### bandit13 to 14
>The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. **Note:** **localhost** is a hostname that refers to the machine you are working on

https://help.ubuntu.com/community/SSH/OpenSSH/Keys
For this level, I needed to learn how to use the private key given and use it to login to the next level, bandit 14. Used `ssh -i sshkey.private bandit14@bandit.labs.overthewir.org -p 2220` which worked. Then I was able to access the passcode below as bandit 14 user in `/etc/bandit_pass/bandit14`. **Note:** when I use `ssh` to login as bandit13 then use `ssh` to login as bandit14 with the private key. When I type `exit`, I go back to bandit 13 then type it again to go back to tuck.
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
#### bandit14 to 15
>The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

https://www.youtube.com/watch?v=7_LPdttKXPc
For this level, I actually used man pages for `nmap` and `nc`. I used nmap to verify that port 30000 was listening on localhost with `nmap -p1-65535 localhost` and `nc -l 127.0.0.1 30000`. Then, I wanted to connect to the port with `nc -n 127.0.0.1 30000`.
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
#### bandit15 to 16
>The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL encryption. 
>**Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…**

https://www.feistyduck.com/library/openssl-cookbook/online/
For this level, I realized I needed a different protocol to connect to port 30001 on localhost. I was able to find a helpful guide on https://ubuntu.com/server/docs/troubleshooting-tls-ssl. I used `openssl s_client -connect 127.0.0.1:30001` and was able to connect to the port with ssl encryption. 
JQttfApK4SeyHwDlI9SXGR50qclOAil1
#### bandit16 to 17
>The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

For this level, I used similar commands from the previous levels in order to identify listening ports on localhost. Then, I identified the port `31790` which returned the RSA key. **Note:** I had trouble with using the private key to access the next level because I did not realize that you need the `BEGIN` and `END` wordage at the top and bottom of the key within the text file. I think, if you use RSA for example, it will generate the correct format which the key will reside within and which ssl protocol will correctly understand and interpret. Also ensure the file permissions are not too weak! `chmod` 
	I saved my private key for bandit17 at: `/tmp/tuktuk92/bandit17/shkey.private`
#### bandit17 to 18
>There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new** 
>**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**

Used `diff --suppress-common-lines` in order to quickly identify the differences in the two files.
hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
#### bandit18 to 19
>The password for the next level is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.

For this level, used the `ssh` command but added parameter `/bin/bash` after the user@host which bypassed the automatic logout, and I was in the home directory. I did not have displayed shell, though I could still type in commands that functioned. 
awhqfNnAbc1naukrpqDYcF95h7HoMTrC
#### bandit19 to 20
>To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

https://en.wikipedia.org/wiki/Setuid
For this level, I used `file` and `ls` to first understand the listed binary within the home directory. Once I ran the binary, it prints that you can run a command as another user. When you run `./bandit20-do id`, you will see the bandit20 user id is also with the bandit19 user id when ran, so I ran `./bandit20-do cat /etc/bandit_pass/bandit20` in order to get the passcode.
VxCazJaVykI6W36BkBU0mJTCM8rR95XT
#### bandit20 to 21
>There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21). 
>**NOTE:** Try connecting to your own network daemon to see if it works as you think

I learned that daemon is another word for server. For this level, I used `nc -l 127.0.0.1 5000` to setup a listening socket on :5000. Then, I used `./suconnect 5000` to establish a connection. This level was straight forward thanks to previous levels and my recent coursework.
NvEJF7oVjkddltPSrdKEFOllh9V1IBcq
#### bandit21 to 22
>A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

This one was relatively straight forward. I used cat on `/etc/cron.d/cronjob_bandit22` which revealed the location of the bin for the cron .sh file. I used `cat /usr/bin/cronjob_bandit22.sh` which showed a command `chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`I then used `cat` on that file in `/tmp` which contained the passcode below. 
WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff
#### bandit22 to 23
>A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed. 
>**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

This one was fun to figure out. Similar to last level, I identified the `cronjob` for bandit 23. Looking at the script, it appeared to create a `md5sum` based off the username then append the password file to a text within `tmp` using the checksum created as the filename! Because I have access to `tmp` on this server, that is where I knew to find the password. 
QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G
#### bandit23 to 24
>A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.
>**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!
>**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

This level was tough because I had a hard time conceptualizing what exactly was going on. I used [this site](https://david-varghese.medium.com/overthewire-bandit-level-23-level-24-3a7efa0e3b99 ) for assistance. I need to slow down and be more deliberate with these CTFs. Studying the man pages and analyzing what exactly is going on. I was able to figure it out.  
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar
#### bandit24 to 25
>A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing. You do not need to create new connections each time

For this level, I need assistance from the previous site again. I need to study bash scripting essentially. I used in the CLI 
`for i in {0000..9999}; do echo “UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ $i”; done | nc localhost 30002` 
The first couple attempts appeared to have bugged, so I tightened the range in order to see if it would work which it did. 
p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d
#### bandit25 to 26
>Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not **/bin/bash**, but something else. Find out what it is, how it works and how to break out of it.

For this level, I had to use the `sshkey` file in the home directory to login to bandit26. I was immediately logged out. Identified the script that was running which used more to terminate once logged in. I had to use `mode` in interactive mode in order to prevent the automatic disconnection. THen, we ented `vim` using the `v` key in `mode`. Then, used `:set shell=/bin/bash` and then `:shell` to enter the bash shell mode. Remember that `vim` can enter commands in the command mode. 
c7GvcKlw9mC7aUQaPx7nwFstuAIBw1o1
#### bandit26 to 27
>Good job getting a shell! Now hurry and grab the password for bandit27!

This level was made to quickly get bandit27 password in order to prevent having to repeat the steps from the previous level. 
YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS
#### bandit27 to 28
>There is a git repository at `ssh://bandit27-git@localhost/home/bandit27-git/repo` via the port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`. 
>Clone the repository and find the password for the next level.

For this level, used `git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo` had to add the port 2220 in order to access the folder through ssh with the right permissions. 
AVanL161y9rsbcJIsFHuw35rjaOM19nR
#### bandit28 to 29
>There is a git repository at `ssh://bandit28-git@localhost/home/bandit28-git/repo` via the port `2220`. The password for the user `bandit28-git` is the same as for the user `bandit28`. Clone the repository and find the password for the next level.
