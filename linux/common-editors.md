# vi/vim

## How to delete the last letters in each line?
Credit [here](https://stackoverflow.com/questions/36264659/how-do-i-delete-the-last-character-in-each-line-using-vim). You could use `:%s/.\{1}$//` to delete 1 character off the end of each line. Hooray! Replace 1 by 2, 3, 4, etc. See the screenshot below.

![demo](images/vim_delete_last_letter.png?raw=true "Delete last letters on every line")

## How to do page up/down or go to the end of the opening file?

I found [this answer](https://superuser.com/questions/335944/vim-in-osx-how-to-do-page-up-page-down-go-to-eol-through-a-vim-file-opened-in-t) is good enough to be memorable.

In navigation mode, `ctrl-f` scrolls down a page and `ctrl-b` scrolls up a page (think "F"orward and "B"ack). `Ctrl-d` scrolls down half a page, and `ctrl-u` scrolls up half a page.

`^`takes you to the beginning of a line, and $ to the end. I know, I know, but there are historical reasons for that.

[Here's](http://www.lagmonster.org/docs/vi2.html) a really good cheat-sheet on vi controls. vi is a little bit arcane, but once you internalize it, it's the quickest and slickest text editor in the world.

Also, there is another great comment from the question on StackExchange - SuperUser:

The easiest and best solution is `n+↓`, where `n` is the number of lines you would like to go down, and `↓` is the down key.

I regularly just do `12+↓` and `22+↓`.

## How to comment and uncomment?
Found [this question](https://stackoverflow.com/questions/1676632/whats-a-quick-way-to-comment-uncomment-lines-in-vim) or [that question](https://stackoverflow.com/questions/2561418/how-to-comment-out-a-block-of-python-code-in-vim) answering all I need.

One way manually
```
:set number
:10,12s/^/#
```
to comment the line 10 to 12.

No plugins or mappings required. Try the built-in "norm" command, which literally executes anything you want on every selected line.

Add # Comments
```
1. shift V to visually select lines
2. :'<,'>:norm i//
```
Remove # Comments
```
1. visually select region as before
2. :'<,'>:norm x
```

# Sublime Text
## Navigation
### page up/down: 
On Mac: Fn + up/down arrow
### Go to begining/end of the document:
On Mac: Fn + left/right arrow

On a Mac keyboard that doesn’t have Home and End keys (which I assume is most but my experience is limited), Fn+Left is the Home key and Fn+Right is the End key.

As such, the key strokes you want out of the box to be able to use Sublime in a remote desktop session originating from a Mac are Fn+Ctrl+Left and Fn+Ctrl+Right to jump to the beginning or end of the file respectively.

Note also that Sublime sees e.g. Fn+Left as Home natively, so any other key binding that uses these keys will work with a similar replacement, and you would use home or end in your key binding if you want to create your own.


[Sublime Text 3 Cheatsheet](https://www.shortcutfoo.com/app/dojos/sublime-text-3-mac/cheatsheet)

[25 tips, tricks and shortcuts with Sublime Text 3](https://generalassemb.ly/blog/sublime-text-3-tips-tricks-shortcuts/)