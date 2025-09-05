# Show full year in the result of `ls` command
Acording to this [answer](https://unix.stackexchange.com/a/415262/30988), `ls -l` will display month, day and **year** - *since*, according to the BSD man page: 

If the modification time of the file is more than 6 months in the past or future, then the year of the last modification is displayed in place of the hour and minute fields.

So, to make sure that year will always be shown, use:

`ls -l --time-style=long-iso` (GNU/Linux)

`ls -lT` will display complete time information in BSD (MacOS)

# Select and copy text in Tmux
In some settings, selecting text in tmux is disable. However, we can press `CMD + R` on MacOS to select any piece of text and `CMD + C` to copy it.

In particular om MacOS, press `Fn` key with left mouse if using `Terminal.app`. Otherwise, press `Opt` key with left mouse if using `iTerms.app`. Note: not press `Shift` key. 

# Delete the content or empty a file
The [simple and fast ways](https://stackoverflow.com/questions/22402054/what-is-the-simplest-way-to-delete-the-contents-of-a-file-in-bash) are 
```bash
> file.txt
true > file.txt # a slight variation

truncate -s 0 file.txt
truncate --size=0 file.txt

cat /dev/null > file.txt
```
