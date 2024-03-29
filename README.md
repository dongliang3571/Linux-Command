# Linux-Command

- `disown` - Remove jobs from current shel
 
   ```bash
   # disown jobID1 jobID2 ... jobIDN
   # -h prevents hangup.
    
   python script.py &
   disown -h
   # now the program will stay in the background even you shut down the terminal
   ```
   
- `lsof` - list open files

   ```bash
   # -i [i] selects the listing of files any of whose Internet address matches the address specified in i.  If no  address  is  specified, this option selects the listing of all Internet and x.25 (HP-UX) network files
   lsof -i udp:portNumber 
   lsof -i :80
   lsof -i :80 | grep LISTEN
   ```

- `netstat` - show network status. netstat will continuously display the information regarding packet traffic on the configured network interfaces.
 
   ```bash
   # --udp/-u show all udp connections
   # --tcp/-u show all tcp connections
   # -a With the default display, show the state of all sockets; normally sockets used by server processes are not shown.
   # -l Print full IPv6 address.
   # -n Show network addresses as numbers (normally netstat interprets addresses and attempts to display them symbolically).
   
   # grep the connection with port 53613
   netstat -at | grep 53613
   ```

- `tcpdump` - dump traffic on a network 
  
  ```bash
  # -A Print each packet (minus its link level header) in ASCII.  Handy for  capturing web pages
  # -i Listen on interface
  # dump traffic on loop back network
  tcpdump -i lo -A | grep "Host:"
  
  # dump traffic on default docker network
  tcpdump -i docker0 -A | grep "Host:"
  
  # dump traffic on for UDP only
  tcpdump -i docker0 -A | grep "Host:"
  
  # any traffic satisfy an expression
  tcpdump -i any "udp port 8125" -w output.pcap
  ```

- `exec` replaces the current shell, no subshell is created. https://www.baeldung.com/linux/exec-command-in-shell-script
  
  ```bash
  # find out current sheel PID
  echo $$
  # Let's say PID is 123
  
  exec sleep 300 # replace current sheel with sleep command
  
  # From a new shell
  ps
  
  # You will see something like
  # user1       53    52  0 23:37 tty2     00:00:00 -bash
  # user1       123    78  0 23:39 tty1     00:00:00 sleep 300
  
  # if exec was not used, a use process will be created
  ```
  
- `scp` secure copy

  ```bash
  scp <current-path> <username>@<ip-address>:/remote-path
  ```
  
- `curl` - transfer a URL

  ```bash
  curl --digest \
    -u{username}:{password} \
    -v \
    -X PUT \
    -H 'Expect: ' \
    -H 'Content-type: application/xml' \
    -d @- \
    http://webapi.ebayclassifieds.com/webapi/partners/{username}/ads/{ext-reference-id} \
        < ad.xml
        
  # --digest (HTTP)  Enables  HTTP  Digest  authentication. This is an authentication scheme that prevents the password from being sent over the wire in clear text.
  # -X defines http method, e.g. POST, PUT
  # -u{username}:{password}
  # -H header
  # -d, --data,  (HTTP) Sends the specified data in a POST request to the HTTP server. This will cause curl to pass the data to the server using the content-type application/x-www-form-urlencoded. If you specify -H "Content-Type: application/json", then you can put -d {"json": "json"}
  # "-d @-" option means that curl will send a POST request with the data it reads from stdin, use "<" to feed a file to stdin
  # "-d @filename" to directly read from file
  
  ```

- `source` will execute commands from a file in the current shell.

  ```bash
  source venv/bin/activate
  ```

- `. `(dot + space) is equivalent to `source`.

  ```bash
  . ./some.sh
  ```

- `cat` concatenate and print files

  ```bash
  cat /dev/null > file.log
  # This will clean up the log even if the file.log is actively being writing by other application
  ```
  
  ```bash
  # The cat <<EOF syntax is very useful when working with multi-line text in Bash, eg. when assigning multi-line string to a shell variable, file or a pipe.
  
  # 1. Assign multi-line string to a shell variable

  $ sql=$(cat <<EOF
  SELECT foo, bar FROM db
  WHERE foo='baz'
  EOF
  )
  
  # The $sql variable now holds the new-line characters too. You can verify with echo -e "$sql".
  
  # 2. Pass multi-line string to a file in Bash

  $ cat <<EOF > print.sh
  #!/bin/bash
  echo \$PWD
  echo $PWD
  EOF
  
  # The print.sh file now contains:
  # #!/bin/bash
  # echo $PWD
  # echo /home/user
  
  # 3. Pass multi-line string to a pipe in Bash

  $ cat <<EOF | grep 'b' | tee b.txt
  foo
  bar
  baz
  EOF
  
  # The b.txt file contains bar and baz lines. The same output is printed to stdout.
  ```
  
  `EOF` is known as a "Here Tag". Basically `<<Here` tells the shell that you are going to enter a multiline string until the "tag" Here. **You can name this tag as you want, it's often EOF or STOP.**
  
  Some rules about the Here tags:

  The tag can be any string, uppercase or lowercase, though most people use uppercase by convention.
  The tag will not be considered as a Here tag if there are other words in that line. In this case, it will merely be considered part of the string. The tag should be by itself on a separate line, to be considered a tag.
  The tag should have no leading or trailing spaces in that line to be considered a tag. Otherwise it will be considered as part of the string.
  
  Example:
  
  ```bash
  $ cat >> test <<HERE
  > Hello world HERE <-- Not by itself on a separate line -> not considered end of string
  > This is a test
  >  HERE <-- Leading space, so not considered end of string
  > and a new line
  > HERE <-- Now we have the end of the string
  ```
  
- `more` collect entire file's content display it in pages(space key for next page, enter key for next line)
- `less` collect enite file's content display it in pages, what it differs from `more` is that it's able to go back to   previous page
  ```bash
  cat ./text.txt
  more ./text.txt
  less ./text.txt
  ```

- `time` shows how long a command takes to run.
  ```bash
  # This will return total time it takes to finish wget command
  time wget https://some.com/download.txt
  ```

- `chmod` is used to change file permissions
  ```bash
  # user group others 
  # rwx  rwx   rwx
  
  # default permissions when create a file is rw-------
  # default permissions when create a directory is rwx------
  
  chmod u+x file.cpp # To give yourself permission to execute a file that you own
  chmod g+r file.cpp # To give members of your group permission to read a file
  chmod o+r file.cpp # To give other permission to read a file
  chmod a+r file.cpp # To all permission to read a file 
  chmod ugo+r file.cpp # To all permission to read a file
  chmod g=u+r file.cpp # assign user's permission to group
  'a' is equivalent to 'ugo'
  chmod a+r -R directory/* # To all permission to all files inside directory
  
  ##################### OR ####################
  # Numerica values for permissions are:
  # 4 = 100 = r-- = read
  # 2 = 010 = -w- = write
  # 1 = 001 = --x = execute
  # 3 = 011 = -wr = write + execute
  
  # change permission for use group and other
  chmod 333 file.cpp # 333 = 011 011 011 = -wx-wx-wx
  # is equivolent to
  chmod ugo+rx file.cpp
  ```
- `chown` is used to change file owner and group
  ```bash
  # Set the user 'daniel' from the group of 'admins' as owners of the directory
  # Note: -R is there when it's a directory
  chown -R daniel:admins important_directory
  ```


- `find` is used to find files in the operating system
  ```bash
  # Format: find <path> -name <filename> -print, <filename> can be wild card but with quotes, eg. '*.cp'
  find / \! -name "*.c" -print # Print out a list of all the files whose names do not end in .c
  find / -newer ttt -user wnj -print # Print out a list of all the files owned by user ``wnj'' that are newer than the file ttt
  find / \! \( -newer ttt -user wnj \) -print # Print out a list of all the files which are not both newer than ttt and owned by ``wnj''
  find / \( -newer ttt -or -user wnj \) -print # Print out a list of all the files that are either owned by ``wnj'' or that are newer than ttt
  find / -type f -exec echo {} \; # Use the echo(1) command to print out a list of all the files
  ```
  
- `cat` is to concatenate and print files
  ```bash
  # create a file named file.c and waiting for you to input content
  # Enter Ctrl-d to stop inputing and save the file
  cat > file.c
  ```
  
- `rm` is to remove files
  ```bash
  rm file.c
  # -i (interactive) option which makes the command prompt you for confirmation that you want to remove each file. Answer: y or n
  rm -i file.c
  ```

- `file` is to examines the content of a file and reports what type of file it is
  ```bash
  file file.sh
  # file.sh: Bourne-Again shell script text executable
  ```

- `head` is to display first 10 lines of the file
- `tail` is to display last 10 lines of the file
  ```bash
  head file.c
  tail file.c
  
  # -n can be used to define numbers of lines you want
  head -n 20 file.c # will give first 20 lines of the file
  
  # Trick to get lines in the middle of the file
  head -n 5 | tail -n 2 # this will give you line 4 and 5
  ```
  
- `ls` is to list the files in a directory
  ```bash
  # -l stands for long listing, to get more information about each file and directory
  # -a stands for all, lists hidden files, "hidden" files that begin with a '.' (dot). All other files and directories are also listed
  # -t sort by modification time, newest first
  # -h make file size human readable, size will end with unit suffixes like Byte, Kilobyte, Megabyte, Gigabyte, Terabyte and Petabyte 
  # -s list file size
  # -G list with colors
  ls -la # will list all files including hidden ones in long listing format
  ```

- `uname` is to report basic information about a computer's software and hardware
  ```bash
  # -s for kernel name (i.e., the default action)
  # -n for network node host name
  # -r for kernel version number and release level
  # -v for date of release of the kernel version
  # -m for machine hardware name
  # -p for CPU type (not available on some systems)
  # -i for general hardware platform
  # -o for operating system
  
  uname -sr # Linux 3.13.0-88-generic
  ```

- `mkdir` is to create directories

  ```bash
  # -m, --mode=MODE, set file mode (as in chmod), not a=rwx - umask
  mkdir -m 777 myFolder
  
  # -p, --parents, no error if existing, make parent directories as needed
  mkdir -p parentFolder/childFolder # error will be generated if no -p added
  
  # -v, --verbose, print a message for each created directory

  # -Z, --context=CTX, set the SELinux security context of each  created  directory  to CTX

  # --help display this help and exit

  # --version, output version information and exit
  ```

- `alias` is to create shortcuts or abbreviations

  ```bash
  alias ll="ls -al"
  alias ls="ls -G"
  ```

- `pushd`, `popd` and `dirs`. The `pushd` command saves the current working directory in memory so it can be returned to at any time, optionally changing to a new directory. The `popd` command returns to the path at the top of the directory stack. This directory stack is accessed by the command `dirs` in Unix

  ```bash
  # your current folder is ~/
  # you do
  pushd ~/.ssh/
  # it take you to ~/.shh, but saves ~/ in the stack
  
  # now you do
  dirs
  # output is ~/
  
  # now you do
  popd

  # it takes you to ~/
  ```

- `ps` - report a snapshot of the current processes.

  Note that "ps -aux" is distinct from "ps aux". The POSIX and UNIX standards require that "ps -aux" print all processes owned by a user named "x", as well as printing all processes that would be selected by the -a option. If the user

  ```bash
  ps aux | grep 'something'
  # it's identical to
  ps -e | grep 'something'
  
  # a show processes for all users
  # u display the process's user/owner
  # x also show processes not attached to a terminal
  # -A Display information about other users' processes, including those without controlling terminals.
  # -e Identical to -A
  # -f Display the uid, pid, parent pid, recent CPU usage, process start time, controlling tty, elapsed CPU usage, and the associated command.  If the -u option is also used, display the user name rather then the numeric uid.  When -o or -O is used to add to the      display following -f, the command field is not truncated as severely as it is in other formats.
  ```
  
  named "x" does not exist, this ps may interpret the command as "ps aux"
  
- `su` (short for substitute user) command makes it possible to change a login session's owner (i.e., the user who originally created that session by logging on to the system) without the owner having to first log out of that session.

  Although su can be used to change the ownership of a session to any user, it is most commonly employed to change the ownership from an ordinary user to the root (i.e., administrative) user, thereby providing access to all parts of and all commands on the computer or system. For this reason, it is often referred to (although somewhat inaccurately) as the superuser command. It is also sometimes called the switch user command.

  ```bash
  #A simplified expression of the syntax of the su command is:
  
  su [options] [commands] [-] [username]

  #The square brackets indicate that the enclosed item is optional. Thus, the simplest way to use the su command is to just type:

  su

  #The operating system assumes that, in the absence of a username, the user wants to change to a root session, and thus the user is prompted for the root password as soon as the ENTER key is pressed. This produces the same result as typing:

  su root

  #If the correct password is provided, ownership of the session is changed to root.

  #Likewise, to transfer the ownership of a session to any other user, the name of that user is typed after su and a space. For example, to change the owner of the current login session to a user named alice, type the following:

  su alice

  #The user will then be prompted for the password of the account with the username alice.
  ```
  
  [good resource](https://unix.stackexchange.com/questions/7013/why-do-we-use-su-and-not-just-su)
  
  `su` - invokes a login shell after switching the user. A login shell resets most environment variables, providing a clean base.

  `su` just switches the user, providing a normal shell with an environment nearly the same as with the old user.

- `du` (abbreviated from disk usage) is a standard Unix program used to estimate file space usage—space used under a particular directory or files on a file system.

  ```bash
  du -s, --summarize # display only a total for each argument, it shows storage usage for each folder
  du -h, --human-readable # human readable, i.e. convert from Byte to KB, MB, GB 
  du -sh *
  
  # 80K	bin
  # 4.0K	buildout.cfg
  # 24K	conf-ins
  # 4.0K	develop-eggs
  # 52M	eggs
  # 24K	etc
  # 4.0K	parts
  # 24K	profiles
  # 552K	src
  # 28K	var
  ```

- `df` (abbreviation for disk free) is a standard Unix command used to display the amount of available disk space for file systems on which the invoking user has appropriate read access. df is typically implemented using the statfs or statvfs system calls.

- `hexdump` ASCII, decimal, hexadecimal, octal dump
  
  ```bash
  hexdump -C yourfile.pdf # dump binary file into both hex and ascii value
  
  # 00000000  25 50 44 46 2d 31 2e 33  0a 25 c4 e5 f2 e5 eb a7  |%PDF-1.3.%......|
  # 00000010  f3 a0 d0 c4 c6 0a 34 20  30 20 6f 62 6a 0a 3c 3c  |......4 0 obj.<<|
  # 00000020  20 2f 4c 65 6e 67 74 68  20 35 20 30 20 52 20 2f  | /Length 5 0 R /|
  # 00000030  46 69 6c 74 65 72 20 2f  46 6c 61 74 65 44 65 63  |Filter /FlateDec|
  # 00000040  6f 64 65 20 3e 3e 0a 73  74 72 65 61 6d 0a 78 01  |ode >>.stream.x.|
  # 00000050  3d 90 41 6e c4 30 08 45  f7 39 c5 3f 01 03 98 18  |=.An.0.E.9.?....|
  # 00000060  bc ee 6c ba cc aa 07 88  ba 9c 56 69 ee 2f 15 db  |..l.......Vi./..|
  # 00000070  99 c8 12 96 be 79 9f 8f  0f 6c 38 c0 79 bc 29 aa  |.....y...l8.y.).|
  # 00000080  28 fe be f1 85 9f 14 bb  d0 1f ba d8 ef fd 85 c7  |(...............|
  # 00000090  e7 4b f0 fc 4d e6 f1 71  0a f6 13 4c ac de ab cb  |.K..M..q...L....|
  
  # -b One-byte octal display.  Display the input offset in hexadecimal, followed by sixteen space-separated, three column, zero-  filled, bytes of input data, in octal, per line.
  
  # -C Canonical hex+ASCII display.  Display the input offset in hexadecimal, followed by sixteen space-separated, two column, hexadecimal bytes, followed by the same sixteen bytes in %_p format enclosed in ``|'' characters.
  
  # -c One-byte character display.  Display the input offset in hexadecimal, followed by sixteen space-separated, three column, space-filled, characters of input data per line.
  
  # -d Two-byte decimal display.  Display the input offset in hexadecimal, followed by eight space-separated, five column, zero-filled, two-byte units of input data, in unsigned decimal, per line.
  ```

- `xxd` make a hexdump or do the reverse. Can do bits, i.e `1`s and `0`s. Default is `hexdump` too

  ```bash
  xxd -b yourfile.pdf # dump binary file into both bits and ascii value
  
  # 00000000: 00100101 01010000 01000100 01000110 00101101 00110001  %PDF-1
  # 00000006: 00101110 00110011 00001010 00100101 11000100 11100101  .3.%..
  # 0000000c: 11110010 11100101 11101011 10100111 11110011 10100000  ......
  # 00000012: 11010000 11000100 11000110 00001010 00110100 00100000  ....4
  # 00000018: 00110000 00100000 01101111 01100010 01101010 00001010  0 obj.
  # 0000001e: 00111100 00111100 00100000 00101111 01001100 01100101  << /Le
  # 00000024: 01101110 01100111 01110100 01101000 00100000 00110101  ngth 5
  # 0000002a: 00100000 00110000 00100000 01010010 00100000 00101111   0 R /
  ```

- double dash `--`

  More precisely, a double dash (--) is used in bash built-in commands and many other commands to signify the end of command options, after which only positional parameters are accepted.

  Example use: lets say you want to grep a file for the string -v - normally -v will be considered the option to reverse the matching meaning (only show lines that do not match), but with -- you can grep for string -v like this:

  ```bash
  grep -- -v file
  ```

- `ssh` OpenSSH SSH client (remote login program)

  http://www.unixwiz.net/techtips/ssh-agent-forwarding.html - agent forwarding

  **port forwading**
  
  https://unix.stackexchange.com/questions/115897/whats-ssh-port-forwarding-and-whats-the-difference-between-ssh-local-and-remot
  
  https://www.ssh.com/ssh/tunneling/example
  
  - **local**: `-L` Specifies that the given port on the local (client) host is to be forwarded to the given host and port on the remote side.

  `ssh -L sourcePort:forwardToHost:onPort connectToHost` means: connect with ssh to `connectToHost`, and forward all connection attempts to the local `sourcePort` to port `onPort` on the machine called `forwardToHost`, which can be reached from the `connectToHost` machine.

  - **remote**: `-R` Specifies that the given port on the remote (server) host is to be forwarded to the given host and port on the local side.

  `ssh -R sourcePort:forwardToHost:onPort connectToHost` means: connect with ssh to `connectToHost`, and forward all connection attempts to the remote `sourcePort` to port `onPort` on the machine called `forwardToHost`, which can be reached from your local machine.

  You can setup configuration in `~/.ssh/config` for ssh

  ```bash
   Host qa1
      User dong
      Hostname <ip_adress>

  # Now you can do 'ssh qa1', it will do 'ssh dong@<ip_adress>'
  ```

  Create ssh tunnel for forwarding

  ```bash
  ssh -N -R 3999:localhost:80 <example_ip>
  ```

  ```bash
  ssh -N -R
  ```

  The `-N` will let you NOT get a shell prompt after you connect, it just hang there, and `-R` is what tells SSH to create the tunnel.


  ```bash
  3999:localhost:80
  ```

  This is where you set the port for the remote server, the local server address, and the port for the local server.

  The first number is the port that you want the remote server to listen on. This can be any number between 1024-65535, and you’ll need to make sure to allow that port in your firewall if you have one set up. Next is the local server address. In almost all cases this will be localhost. And finally, the last number is the port that your local web server is listening on.

  ```bash
  <example_ip>
  ```

  The last part of the command is where you specify your user that has SSH access to the server and the address of the remote server.

  If you already have a domain name setup in DNS for the server, you’ll be able to use that to access the tunnel. Otherwise, you’ll need to use the server’s IP address.

  Now that you have an SSH tunnel open, going to the remote server address with the forwarded port in your browser, e.g. example.com:3999, should allow you to view your local website or app from anywhere with an internet connection.

  If you have `~/.ssh/config` set up with `LocalForward` like below

  ```bash
   Host qa1
      User dong
      Hostname <ip_adress>
      LocalForward 5000 <ip_adress>:<port_number>

  ```

  you can do

  ```bash
  ssh -N qa1
  ```

  or run it in background

  ```bash
  ssh -N -f qa1
  ```

  Both `ssh -N -f qa1` and `ssh -N qa1 &` will run the job in background, but the difference between two is that `ssh -N qa1 &` will attach to terminal, if terminal is closed, tunnel is closed too. `ssh -N -f qa1` will not attach the tunnel to the terminal.

- `lsb_release` print distribution specific information

  ```bash
  $ lsb_release -a
  LSB Version:	:base-4.0-amd64:base-4.0-noarch:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
  Distributor ID:	CentOS
  Description:	CentOS release 6.5 (Final)
  Release:	6.5
  Codename:	Final
  displays all of the above information.
  ```
  
- `netstat` show network status

```bash
netstat --listen # list all listening port
```

- `top`

   ```bash
   top # a process monitor
   
   top -o cpu # sort the process by cpu usage
   ```

- `grep` - file pattern searcher

  ```bash
  grep 'ip' buildout.cfg # find the lines that contains keyword 'ip'
  ```

- `awk`- pattern scanning and processing language
  
  **Usage**
  ```bash
  awk '/search_pattern/ { action_to_take_on_matches; another_action; }' file_to_parse
  ```
  You can omit either the search_pattern or the action portion from any awk command. By default, the action taken if the "action" portion is not given is "print". This simply prints all lines that match.
  
  ```bash
  awk '/c/' /etc/fstab # print out the lines with "c" in it
  ```
  
  It's same as:
  
  ```bash
  awk '/c/ {print}' /etc/fstab # print out the lines with "c" in it
  ```
  
  we can use the action section to specify which pieces of information we want to print. For instance, to print only the first column, we can type:
  
  ```bash
  awk '{print $1}' /etc/fstab # print out all lines in /etc/fstab file
  ```
  
  We can reference every column (as delimited by whitespace) by variables associated with their column number. The first column can be referenced by `$1` for instance. The entire line can by referenced by `$0`

- `xargs` - construct argument list(s) and execute utility

  ```bash
  # A scenario where you want to kill a bunch of process that contains same keyword
  
  ps aux 
  
  # USER   PID   %CPU  .....  CMD
  # Dong   123   2.4   .....  java test
  # Dong   234   4.3   .....  java test2
  
  ps aux | awk '/test/ {print $2}'
  
  # 123
  # 234
  
  ps aux | awk '/test/ {print $2}' | xargs kill -9
  
  # Now xargs will be a argument set that contains all PIDs which generated from awk and 'kill -9' will take each pid as argument then  kill all process that's selected
  ```

- `wc` q- word, line, character, and byte count

  ```bash
  wc buidlout.cfg
  #        5      48    2697 buildout.cfg
  
  # -c - number of bytes
  # -l - number of lines
  # -m - number of characters
  # -w - number of words
  ```

- `sed` stream editor

delete one or more lines from a file

```bash
sed '{[/]<n>|<string>|<regex>[/]}d' <fileName>       
sed '{[/]<adr1>[,<adr2>][/]d' <fileName>

# /.../=delimiters 

# n = line number 

# string = string found in in line 

# regex = regular expression corresponding to the searched pattern 

# addr = address of a line (number or pattern ) 

# d = delete 
```

Here are some examples of how to use the above syntax. 

Use the following code to remove the third line: 

`sed '3d' fileName.txt`

Remove the line containing the string "awk," by using: 

`sed '/awk/d' filename.txt`

You can remove the last line by typing in: 

`sed '$d' filename.txt`

Or remove all empty lines through: 

```bash
sed '/^$/d' filename.txt       
sed '/./!d' filename.txt
```

Remove the line matching by a regular expression (by eliminating one containing digital characters, at least 1 digit, located at the end of the line): 

`sed '/[0-9/][0-9]*$/d' filename.txt`

Remove the interval between lines 7 and 9: 

`sed '7,9d' filename.txt `

The same operation as above but replacing the address with parameters: 

`sed '/-Start/,/-End/d' filename.txt`

The above examples are only changed at the display of the file (stdout1= screen). 

For permanent changes to the old versions (<4) use a temporary file for GNU sed using the "-i[suffix]": 

`sed -i".bak" '3d' filename.txt`

- `read` - take user input

A no-argument call of read fetches a single line from the standard input stream and assigns it to the REPLY built-in variable:

```bash
$ read
baeldung is a cool tech site # what we type
$ echo $REPLY
baeldung is a cool tech site
```

Let’s now specify some variables to store the results:

```bash
$ read input1 input2 input3
baeldung is a cool tech site # what we type
$ echo "[$input1] [$input2] [$input3]"
[baeldung] [is] [a cool tech site]
```

If the number of variables is lower than the words obtained, all the remaining words and their delimiters are crammed in the last variable.

By default the read command splits the input into words, considering the <space>,<tab> and <newline> characters as word delimiters.

We can pass the input on multiple lines using the special <backslash> character:

```bash
$ read input1 input2 input3
baeldung \ # what 
is a cool \ # we 
tech site   # type
$ echo "[$input1] [$input2] [$input3]"
[baeldung] [is] [a cool tech site]
```

### Bash 

`-z string` True if the string is null (an empty string)

```bash
String=''   # Zero-length ("null") string variable.

if [ -z "$String" ]
then
  echo "\$String is null."
else
  echo "\$String is NOT null."
fi     

# $String is null.
```

### difference betweet `` `stuff` `` and `$(stuff)`

```bash
echo `whoami`

echo $(whoami)
```

The old-style backquotes `` ` ` `` do treat backslashes and nesting a bit different.

The new-style `$()` interprets everything in between ( ) as a command.

```bash
echo $(uname | $(echo cat))
Linux

echo `uname | `echo cat``
bash: command substitution: line 2: syntax error: unexpected end of file
echo cat
```

works if the nested backquotes are escaped:

```bash
echo `uname | \`echo cat\``
Linux
```

backslash fun:

```bash
echo $(echo '\\')
\\

echo `echo '\\'`
\
```

The new-style `$()` applies to all POSIX-conformant shells.
As mouviciel pointed out, old-style `` ` ` `` might be necessary for older shells.

Apart from the technical point of view, the old-style `` ` ` `` has also a visual disadvantage:

Hard to notice: I like `$(program)` better than `` `program` ``

Easily confused with a single quote: `` '`'`''`''`'`''`' ``

Not so easy to type (maybe not even on the standard layout of the keyboard)

### Parameter expansion(conditional operator)

According to the section on "Parameter Expansion" in the bash man page, this means "use the default value if the parameter is unset." So for example,

```bash
${PID-/run/unicorn.pid}
```

equals `$PID if $PID is set, otherwise /run/unicorn.pid`.


`${A-something}` only means an **unset** variable. Not an empty one.
And to put to comparison with `:-`, which will use default value (the one after minus sign) if variable is **unset or null** (as in empty string).

## ShortCut

### Searching Through The Command History

  ```bash
  # To search backward in the history for a particular string, type C-r. Typing C-s searches forward through the histor
  # ie. C-r goes in direction of from the newest histroy to the oldest
  # ie. C-s goes in direction of from the oldest to the newest

  # example:
  (reverse-i-search)`ss': ssh -vvv lp123
  ```

###  .bash_profile, .profile, .bashrc, zshrc, .zlogin

[Good resource](https://superuser.com/questions/183870/difference-between-bashrc-and-bash-profile)

Traditionally, when you log into a Unix system, the system would start one program for you. That program is a shell, i.e., a program designed to start other programs. It's a command line shell: you start another program by typing its name. The default shell, a Bourne shell, reads commands from `~/.profile` when it is invoked as the login shell.

Bash is a Bourne-like shell. It reads commands from `~/.bash_profile` when it is invoked as the login shell, and if that file doesn't exist¹, it tries reading `~/.profile` instead.

You can invoke a shell directly at any time, for example by launching a terminal emulator inside a GUI environment. If the shell is not a login shell, it doesn't read `~/.profile`. When you start bash as an interactive shell (i.e., not to run a script), it reads `~/.bashrc` (except when invoked as a login shell, then it only reads `~/.bash_profile` or `~/.profile`.

Therefore:

`~/.profile` is the place to put stuff that applies to your whole session, such as programs that you want to start when you log in (but not graphical programs, they go into a different file), and environment variable definitions.

`~/.bashrc` is the place to put stuff that applies only to bash itself, such as alias and function definitions, shell options, and prompt settings. (You could also put key bindings there, but for bash they normally go into `~/.inputrc`.)

`~/.bash_profile` can be used instead of `~/.profile`, but it is read by bash only, not by any other shell. (This is mostly a concern if you want your initialization files to work on multiple machines and your login shell isn't bash on all of them.) This is a logical place to include `~/.bashrc` if the shell is interactive. I recommend the following contents in `~/.bash_profile`:

```
if [ -r ~/.profile ]; then . ~/.profile; fi
case "$-" in *i*) if [ -r ~/.bashrc ]; then . ~/.bashrc; fi;; esac
```

On modern unices, there's an added complication related to `~/.profile`. If you log in in a graphical environment (that is, if the program where you type your password is running in graphics mode), you don't automatically get a login shell that reads `~/.profile`. Depending on the graphical login program, on the window manager or desktop environment you run afterwards, and on how your distribution configured these programs, your `~/.profile` may or may not be read. If it's not, there's usually another place where you can define environment variables and programs to launch when you log in, but there is unfortunately no standard location.

Note that you may see here and there recommendations to either put environment variable definitions in `~/.bashrc` or always launch login shells in terminals. Both are bad ideas. The most common problem with either of these ideas is that your environment variables will only be set in programs launched via the terminal, not in programs started directly with an icon or menu or keyboard shortcut.

¹ For completeness, by request: if `.bash_profile` doesn't exist, bash also tries `.bash_login` before falling back to `.profile`. Feel free to forget it exists.


### stdout, stdin, stderr

It is good practice to redirect all error messages to `stderr`, while directing regular output to stdout. It is beneficial to do this because anything written to `stderr` is not buffered, i.e., it is immediately written to the screen so that the user can be warned immediately.

Python example:

```python
HOST="www.example.org"
# Ports are handled in ~/.ssh/config since we use OpenSSH
COMMAND="uname -a"

ssh = subprocess.Popen(["ssh", "%s" % HOST, COMMAND],
                       shell=False,
                       stdout=subprocess.PIPE,
                       stderr=subprocess.PIPE)
result = ssh.stdout.readlines()
if result == []:
    error = ssh.stderr.readlines()
    print >>sys.stderr, "ERROR: %s" % error  # Note: the '>>' will redirect error to stderr
else:
    print result
```

### `&>` and `>&` and `>`

`STDIN` is represented by `0`, `STDOUT` by `1`, and STDERR by `2.

```
There  are  two  formats  for  redirecting standard output and standard
   error:

          &>word
   and
          >&word

   Of the two forms, the first is preferred.  This is semantically equiva-
   lent to

          >word 2>&1
```

That is , 
- `command 1> file` redirects the stdout to a file, stderr will show 
- `command 2> file` redirects the stderr to a file, stdout will show
- `command &> file` is redirects both stdout and stderr to file, neither stderr or stdout will show, that is same as `command > file 2>&1`


# Terminal shortcut

[Shortcuts to move faster in Bash command line](http://teohm.com/blog/shortcuts-to-move-faster-in-bash-command-line/)

[bash shortcuts](http://www.skorks.com/2009/09/bash-shortcuts-for-maximum-productivity/)

### Basic moves

- Move back one character. `Ctrl + b`
- Move forward one character. `Ctrl + f`
- Delete current character. `Ctrl + d`
- Delete previous character. `Backspace`
- Undo. `Ctrl + -`

### Moving faster and

- Move to the start of line. `Ctrl + a`
- Move to the end of line. `Ctrl + e`
- Move forward a word. `Meta + f` (a word contains alphabets and digits, no symbols)
- Move backward a word. `Meta + b`
- Move between start of command line and current cursor position (and back again). `Ctrl + xx`
- Clear the screen. `Ctrl + l`

**What is Meta?** **Meta** is your Alt key, normally. For Mac OSX user, you need to enable it yourself. Open Terminal > Preferences > Settings > Keyboard, and enable Use option as meta key. Meta key, by convention, is used for operations on word. **By default, you can use `ESC` instead of Meta(alt) key.**

### Cut, paste (‘Kill and yank’ for old schoolers) and editing words

- Cut from cursor to the end of line. `Ctrl + k`
- Cut from cursor to the end of word. `Meta + d`
- Cut from cursor to the start of word. `Meta + Backspace`
- Cut from cursor to previous whitespace. `Ctrl + w`
- Cut from cursor to the begining of the line. `Ctrl + u`
- Paste the last cut text. `Ctrl + y`
- Loop through and paste previously cut text. `Meta + y` (use it after `Ctrl + y`)
- Loop through and paste the last argument of previous commands. `Meta + .`
- Capitalize first character. `Meta + c`
- Capitalize the whole word. `Meta + u`
- Swap current word with previous. `Meta + t`

### Search the command history

- Search as you type. `Ctrl + r` and type the search term; Repeat `Ctrl + r` to loop through results.
- Search the last remembered search term. `Ctrl + r` twice.
- End the search at current history entry. `Ctrl + j`
- Cancel the search and restore original line. `Ctrl + g` or `ESC`

### Others

- stops the output to the screen (for long running verbose command). `Ctrl + s` 
- allow output to the screen (if previously stopped using command above). `Ctrl + q` 
- terminate the command. `Ctrl + c` 
- suspend/stop the command. `Ctrl + z` 

### Bash Bang (!) Commands

Bash also has some handy features that use the ! (bang) to allow you to do some funky stuff with bash commands.

- `!!` – run last command
- `!blah` – run the most recent command that starts with ‘blah’ (e.g. !ls)
- `!blah:p` – print out the command that !blah would run (also adds it as the latest command in the command history)
- `!$` – the last word of the previous command (same as Alt + .)
- `!$:p` – print out the word that !$ would substitute
- `!*` – the previous command except for the last word (e.g. if you type ‘find some_file.txt /‘, then !* would give you ‘find some_file.txt‘)
- `!*:p` – print out what !* would substitute
