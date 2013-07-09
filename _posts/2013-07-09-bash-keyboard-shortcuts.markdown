---
layout: post
title: Blog
subtitle: Bash Keyboard Shortcuts
category: posts
---

I just recently discovered that bash has keyboard shortcuts to help you move around and edit things faster. Here's the list of some of my favorites.

## Keyboard Shortcuts

- <code>Ctrl + A</code>: Go to the beginning of the line you are currently typing on
- <code>Ctrl + E</code>: Go to the end of the line you are currently typing on
- <code>Ctrl + F</code>: Forward one character.
- <code>Ctrl + B</code>: Backward one character.
- <code>Meta + F</code>: Move cursor forward one word on the current line
- <code>Meta + B</code>: Move cursor backward one word on the current line
- <code>Ctrl + P</code>: Previous command entered in history
- <code>Ctrl + N</code>: Next command entered in history
- <code>Ctrl + L</code>: Clears the screen, similar to the clear command
- <code>Ctrl + U</code>: Clears the line before the cursor position. If you are at the end of the line, - clears the entire line.
- <code>Ctrl + H</code>: Same as backspace
- <code>Ctrl + R</code>: Lets you search through previously used commands
- <code>Ctrl + C</code>: Kill whatever you are running
- <code>Ctrl + D</code>: Exit the current shell
- <code>Ctrl + Z</code>: Puts whatever you are running into a suspended background process. fg restores it.
- <code>Ctrl + W</code>: Delete the word before the cursor
- <code>Ctrl + K</code>: Kill the line after the cursor
- <code>Ctrl + Y</code> : Yank from the kill ring
- <code>Ctrl + _</code>: Undo the last bash action (e.g. a yank or kill)
- <code>Ctrl + T</code>: Swap the last two characters before the cursor
- <code>Meta + T</code>: Swap the last two words before the cursor

The list goes on and if you want to know more take a look at [bash reference][]

## Bonus Tip

To be able to search through bash command history using up and down arrows add following script to your <code>~/.inputrc</code> file:

{% highlight bash %}
"\e[A": history-search-backward
"\e[B": history-search-forward
set show-all-if-ambiguous on
set completion-ignore-case on
{% endhighlight %}

Like [the single most useful thing in bash][] explains:

> type "cd /" and press the up arrow and you'll search through everything in your history that starts with "cd /".

[bash reference]: http://www.gnu.org/software/bash/manual/bashref.html#Command-Line-Editing
[the single most useful thing in bash]: https://coderwall.com/p/oqtj8w