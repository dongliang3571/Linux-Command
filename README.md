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
  
  ```


- `find` is used to find files in the operating system
  ```bash
  find / \! -name "*.c" -print # Print out a list of all the files whose names do not end in .c
  find / -newer ttt -user wnj -print # Print out a list of all the files owned by user ``wnj'' that are newer than the file ttt
  find / \! \( -newer ttt -user wnj \) -print # Print out a list of all the files which are not both newer than ttt and owned by ``wnj''
  find / \( -newer ttt -or -user wnj \) -print # Print out a list of all the files that are either owned by ``wnj'' or that are newer than ttt
  find / -type f -exec echo {} \; # Use the echo(1) command to print out a list of all the files
  ```
  
