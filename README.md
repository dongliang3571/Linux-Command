# Unix-Command

- `source` will execute commands from a file in the current shell.

  ```bash
  source venv/bin/activate
  ```

- `. `(dot + space) is equivalent to `source`.

  ```bash
  . ./some.sh
  ```

- `cat` collect entire file's content and dump onto screen.
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
