# vi/vim

## How to delete the last letters in each line?
Credit [here](https://stackoverflow.com/questions/36264659/how-do-i-delete-the-last-character-in-each-line-using-vim). You could use `:%s/.\{1}$//` to delete 1 character off the end of each line. Hooray! Replace 1 by 2, 3, 4, etc. See the screenshot below.

![demo](images/vim_delete_last_letter.png?raw=true "Delete last letters on every line")
