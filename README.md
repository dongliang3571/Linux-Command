# Unix-Command

`source` will execute commands from a file in the current shell.

```bash
source venv/bin/activate
```

`. `(dot + space) is equivalent to `source`.

```bash
. ./some.sh
```

- `cat` collect entire file's content and dump onto screen.
- `more` collect entire file's content display it in pages(space key for next page, enter key for next line)
- `less` collect enite file's content display it in pages, what it differs from `more` is that it's able to go back to previous page
```bash
cat ./text.txt
more ./text.txt
less ./text.txt
```

`time` shows how long a command takes to run.
```bash
# This will return total time it takes to finish wget command
time wget https://some.com/download.txt
```

