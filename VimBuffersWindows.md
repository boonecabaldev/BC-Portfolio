
# Mastering Vim: Buffers, Windows, and Your Text Editing Arsenal

### Introduction

In this tutorial we will cover the basics of working with buffers and windows (called panes) in Vim using an example-driven approach. To get the most out of it, I strongly recommend that you keep Vim open and follow each step. Do not skip any steps. Just be patient and do each step, one at a time. Actively doing it will reinforce the concepts, making it easier to learn.

## Buffers: Your Textual Workspaces

A buffer is an area in memory that stores text. Every time you open a file, its contents are read into a buffer, allowing you to make changes to that buffer without affecting the original file.

### Viewing Buffers

Let's begin by opening vim. By default you will have one unnamed buffer.  You can view all your buffers using `:ls` or `:buffers`.

Use `:ls`.

```vim
:ls
  1 %a   "[No Name]"                    line 1
```

The buffer list produced by `:ls` uses the following format:

```text
Number Indicator    Name                          Line
```

```vim
     1        %a   "[No Name]"                    line 1
```

All of these fields are self-explanatory except for Indicator. Here is an excerpt from the vim help for `:ls`:

```text
u    an unlisted buffer (only displayed when [!] is used)
        |unlisted-buffer|
 %   the buffer in the current window
 #   the alternate buffer for ":e #" and CTRL-^
 a   an active buffer: it is loaded and visible
 h   a hidden buffer: It is loaded, but currently not
        displayed in a window |hidden-buffer|
  -  a buffer with 'modifiable' off
  =  a readonly buffer
  R  a terminal buffer with a running job
  F  a terminal buffer with a finished job
  ?  a terminal buffer without a job: `:terminal NONE`
  +  a modified buffer
  x  a buffer with read errors
```

The Indicator in the above `:ls` output is `%a`, which specifies that the buffer is active (`a`) and in the current window or pane (`%`). `+` is useful; it tells you if a buffer is modified. We'll cover another useful one, `#`, later.

### `:ls` Helper Function

Throughout this article we will be issuing command-line commands followed by the `:ls` command like `:bd | ls`. As a convenience I recommend adding the following function to your `.vimrc`:

```vim
" You must specify 'normal' for all normal-mode commands to work.
function! ExeCmdLs(cmd)
  if a:cmd =~ '^normal'
    exe a:cmd
    ls
  elseif empty(a:cmd)
    ls
  else
    exe a:cmd .. ' | ls'
  endif
endfunction

nnoremap <space>c :call ExeCmdLs('')<left><left>
```
The above code includes a useful normal-mode mapping, `<space>c`, which places you in command-line mode, types `:call ExeCmdLs('_')`, and places your cursor at the `_`.

For the rest of the article where ever your see `:[command] | ls`, this translates to using the `<space>c[command]` macro.

### Saving Buffers to Files

Before we can create a new buffer, we need to save the active buffer to a file, You can do this using `:w`. Add the text `File a.txt` to the buffer, and then use `:w a.txt | ls`.

```vim
"a.txt" [New] 1L, 11B written
  1 %a   "a.txt"                        line 1
```

Now it has a name of `a.txt`, a number of 1, and an indicator of `%a`,  telling you it is the active buffer in the current window.

In addition to `:w`, you can save all open buffers using `:wa`, which stands for "write all."

### Creating Buffers

`:enew` creates a new, unnamed buffer. Use `:enew | ls`:

```vim
:call ExeCmdLs('enew')
  1 #    "a.txt"                        line 1
  3 %a   "[No Name]"                    line 1
```

We have named buffer `a.txt` (1) and unnamed buffer (3); buffer 3 is the active buffer. Enter the text `File b.txt`, then use `:w b.txt | ls`.

```vim
"b.txt" [New] 1L, 11B written
  1      "a.txt"                        line 1
  3 %a   "b.txt"                        line 1
```

Now we have two files to work with.

### Deleting Buffers

`:bd` deletes the active buffer. Delete buffer `b.txt` (3) using `:bd | ls`.

```vim
"a.txt" 1L, 11B
  1 %a   "a.txt"                        line 1
```

Now we're back to buffer `a.txt`.

### Opening Files Into Buffers

`:e` opens a file in the active buffer. Open `b.txt` using `:e b.txt | ls`.

```vim
"b.txt" 1L, 11B
  1 #    "a.txt"                        line 1
  3 %a   "b.txt"                        line 1
```

> #### The `#` Indicator
> 
> Notice that buffer `a.txt` (1) has an indicator of `#`. This is the
> last edited buffer. You can use `:e #` to toggle between the current
> and last edited buffers. Use `:e # | ls` a few times and notice how
> the indicators change.

### Navigating Between Buffers

The following commands allow you to navigate between buffers:

- `:[N]bnext [N]` or `:[N]bn [N]`: move to next buffer, where N is the buffer number.
- `:[N]bprev [N]` or `:[N]bp [N]`: move to previous buffer, where N is the buffer number.
- `:[N]buffer [N]` or `:[N]b [N]`: move to specified buffer N, where N is the buffer number.
- `:bf[irst]`: move first buffer in buffer list.
- `:bl[ast]`: move to last buffer in buffer list.
- `:buffer [N]` or `:b [N]`: move to buffer `[N]`.

From the previous `:ls` command, we have buffers `a .txt` (1) and `b.txt` (3) open, with `b.txt` (3) being the active buffer. Use `:bp | ls` to navigate to buffer `a.txt` (1).

```vim
"a.txt" 1L, 10B
  1 %a   "a.txt"                        line 1
  3 #    "b.txt"                        line 1
```

The `%a` indicator tells you that buffer `a.txt` (1) is the active buffer. Now use `:bp | ls`.

```vim
"b.txt" 1L, 11B
  1 #    "a.txt"                        line 1
  3 %a   "b.txt"                        line 1
```

Buffer `b.txt` (3) is now the active. 

You can navigate to a specific buffer using `:buffer` or `:b`. Use `:b 1 | ls` to navigate to buffer `a.txt` (1).

```vim
"a.txt" 1L, 10B
  1 %a   "a.txt"                        line 1
  3 #    "b.txt"                        line 1
```

### Iterating Over Buffers

 `:bufdo [command]` allows you to perform a command over each buffer. Use `:bufdo bd` to delete all buffers.

```vim
:ls
  3 %a   "[No Name]"                    line 1
```

One use of `:bufdo s/File/&:/ | update` is to perform search-and-replace across multiple buffers. Open `a.txt` and `b.txt` using `:e a.txt | e b.txt | ls`.

```vim
"b.txt" 1L, 11B
  1 #    "a.txt"                        line 1
  2 %a   "b.txt"                        line 1
```

The contents of `a.txt` and `b.txt` are `File a.txt` and `File b.txt`, respectively. Let's search-and-replace `File` with `File:`. Use `:bufdo! s/File/&:/ | update`. Now, if you navigate between buffers using `:bn` and `:bp`, you will see both files have been changed.

Using `:bufdo!` prevents the `No write since last change` error.

> `:update` is a smarter version of `:w` in that it only saves the file if changes have been made.

### Exiting Vim

You can exit vim using the following commands:

- `:q`: Quit the current window, which can be either the pane or the Vim window. Fails when changes have been made.
- `:wq`: Write current file and close window. Quit if last edit. Writing fails when buffer is unnamed.
- `:x`: Like `:wq`, except write ony when changes have been made.

Let's look at a caveat of using quit commands. Save the active buffer and quit Vim using `:wq`. Now reopen `a.txt` and `b.txt` in Vim using the following command:

```bash
vim a.txt b.txt
```

Using `:ls` shows both files are open.

```vim
:ls
  1 %a   "a.txt"                        line 1
  2      "b.txt"                        line 0
```

Try to quit Vim using `:q`. It will say `E173: 1 more file to edit`. It turns out you have to activate every buffer before you can quit.  Use `:bn | q` to navigate to the next buffer `b.txt` (2) and then quit. Now you shouldn't have this problem.

> I noticed this problem when I tried to save all buffers and quit Vim using `:wa | q`, which failed.

### Buffer States

Buffer states are complex. It took me a while to understand them. Let's cover each of the six buffer states.

#### Unlisted

An unlisted buffer does not appear in the buffer list. You can utilize an unlisted buffer whenever you want a temporary area in which to edit text. Once you're done, you can delete the buffer and discard the changes. The following code creates an unlisted buffer:

```vim
" Create listed buffer
:enew 
" Make active buffer unlisted
:setlocal nobuflisted
```

`:ls!` and `:ls u` display all buffers, both listed and unlisted. Using `:ls!` produces the following output:

```vim
:ls!
  1 #    "a.txt"                        line 1
  2      "b.txt"                        line 1
  3u%a   "[No Name]"                    line 1
```
The `u` portion of buffer 3's indicator means unlisted.
#### Inactive

An inactive buffer is is not displayed in any pane. For example, using `:new` creates a new pane and buffer, thereby making the previously active buffer inactive.

#### Active

An active buffer is any buffer currently displayed in any pane. Therefore, you can have multiple active buffers opened in multiple panes.

#### Hidden

You can have multiple buffers loaded in memory, but you can only display one buffer per pane. In other words, any buffer not displayed in any pane is hidden. You can make a buffer hidden using `:hide`.

#### Loaded

A loaded buffer contains the contents of a file. For instance, if you open a file using `:edit a.txt`, that buffer is loaded.

#### Unloaded

`:bunload N` unloads a buffer and its contents from memory, but it still appears in the buffer list.

## Windows (Panes): Viewing and Editing in Parallel

While buffers store your text in memory, windows (panes) are sections within the Vim window that display it. Splitting the Vim window into multiple panes lets you simultaneously view and edit different buffers, streamlining your workflow without constantly switching between files. 

### Splitting and Navigating

You can create panes using the following commands, each of which splits the Vim window into a new pane.

- `:new`: create a new pane and start editing an unnamed buffer.
- `:split {file}` `sp`: create a new pane and start editing file {file} in it. Works almost like `:split | edit {file}` when `{file}` is supplied.
- `:vsplit` `vs`: works like `:split` but split vertically.
- `:vnew`: 

> `:q` closes the active pane but leaves the buffer in memory. If you want to close both the pane and its buffer, use `:bd`.

#### Demonstrating `:new`

Close and reopen Vim, and then use `:new | ls` to create a bottom-docked pane. Observe how both panes contain the same buffer.

```vim
:call ExeCmdLs('new')
  1 #a   "[No Name]"                    line 1
  2 %a   "[No Name]"                    line 1
```

> Whichever pane contains the cursor is the active pane.

#### Demonstrating `:vsplit`

Use `:vs` to create a top-docked pane. This divides the bottom pane vertically into two sub-panes. Pretty cool, isn't it?

#### Demonstrating `:split`

Let's create a different layout. Reset the buffers using `:bd | ls`.

```vim
:ls
  1 %a   "[No Name]"                    line 1
```

Use `:vs` to split the Vim vertically into left- and right-docked panes. Next, use `:sp` to split the right-docked pane horizontally into two sub-panes.

#### Settings for `:new`, `:sp`, and `:vs`

The following options control the behavior of `:split` and `:vsplit`.

- `splitbelow` or `sb`: When on, splitting a pane will put the new pane below the current one.
- `splitright` or `spr`: When on, splitting a pane will put the new pane right the current one.

> You can unset a Vim option by prepending the setting with `no`. For example, unset `splitbelow` using `:set nosplitbelow`.

#### Navigating Between Panes

You can navigate between panes using `:wincmd {arg}`, with `{arg}` being a normal-mode movement command like `k` (up), `j` (down), `l` (right), and `h` (left). Alternatively, you can use the hotkeys `:Ctrl-w h/j/k/l`.

> It's important to understand that both buffers and windows have numbers which identify them, and you can supply them as arguments to various Vim functions. Furthermore, understanding the difference between navigating between panes versus buffers will help you avoid getting lost when you have lots of open panes open.

#### Iterating Over Panes

Similar to `:bufdo`, `:windo` lets you invoke a command for each buffer. For instance,, use `:windo echo winnr()` to display the pane number of each pane.

#### Deleting Panes

`:x`, `:bd`, `:q`, and `:wq` close windows. Understand that you can delete a buffer or a buffer and a pane, but you can't delete a pane without deleting a buffer.

Close and then reopen Vim with `a.txt`. Next, use `:vs b.txt | ls` to open `b.txt` into a right-docked pane.

```vim
"b.txt" 1L, 11B
  1 #a   "a.txt"                        line 0
  2 %a   "b.txt"                        line 1
```

Now the Vim window is vertically divided between left- and right-docked panes. Each pane now has an active buffer, with buffer `b.txt` in the current window (`%`).

> Recall that the `%` indicator defines the active window.

`:close` closes the active pane without deleting its buffer. Use `:close | ls` and notice how closing a pane doesn't delete a buffer.

As a final example, use `:vs b.txt` to open `b.txt` into a right-docked pane and then `:bd | ls` to delete  it.

```vim
:call ExeCmdLs('bd')
  1 %a   "a.txt"                        line 1
``` 
Notice how this deletes buffer `b.txt` and its pane. Again, it is important the distinguish between deleting panes and buffers.

## Example Using Buffers and Panes

Let's walk through a comprehensive example using everything we've learned thus far. Begin by opening `a.txt`, `b.txt`, and `c.txt` in Vim using the bash command `vim a.txt b.txt c.txt`, and then use `:ls`. Observe the indicators as you use the following commands.

```vim
:ls
  1 %a   "a.txt"                        line 1
  2      "b.txt"                        line 0
  3      "c.txt"                        line 0
```

Use `:vnew | new | ls` to vertically split the Vim window into left- and right-docked panes, with the right-docked pane split horizontally between bottom- and top-docked sub-panes.

```vim
:vnew
:new
:ls
  1  a   "a.txt"                        line 0
  2      "b.txt"                        line 0
  3      "c.txt"                        line 0
  4 #a   "[No Name]"                    line 0
  5 %a   "[No Name]"                    line 1
```

Use `:b c.txt | ls` to navigate to buffer `c.txt` and notice how this deletes buffer 5.

```vim
:b c.txt
:ls
  1  a   "a.txt"                        line 0
  2      "b.txt"                        line 0
  3 %a   "c.txt"                        line 1
  4  a   "[No Name]"                    line 0
```

Use `:winc k | ls` to move the cursor to the top-right pane.

```vim
:winc k
:ls
  1 #a   "a.txt"                        line 0
  2      "b.txt"                        line 0
  3  a   "c.txt"                        line 1
  4 %a   "[No Name]"                    line 1
```

Use `b b.txt | ls` to navigate to buffer `b.txt`.

```vim
:b b.txt
:ls
"b.txt" 1L, 11B
  1  a   "a.txt"                        line 0
  2 %a   "b.txt"                        line 1
  3  a   "c.txt"                        line 1
```

## Simplifying Commands

Let's consolidate some commands. Using  `:vs b.txt | sp c.txt | ls` achieves the same result as the previous example.

```vim
:ls
  1  a   "a.txt"                        line 0
  2 #a   "b.txt"                        line 1 <== Alternate buffer
  3 %a   "c.txt"                        line 1 <== Active buffer
```

Notice that buffer `b.txt` (2) has the `#` indicator, which represents the alternate toggle buffer. Use `:e # | ls` to toggle to it.

```vim
"b.txt" 1 line --100%-- ((1) of 3)
  1  a   "a.txt"                        line 0
  2 %a   "b.txt"                        line 1 <== Active buffer
  3 #    "c.txt"                        line 1 <== Alternate buffer
```

Now buffer `c.txt` (3) is the alternate buffer.

Use `:e # | ls` to toggle back to buffer `b.txt` (2), and then use `:bd | bd | ls` to delete buffers `b.txt` (2) and `c.txt` (3). Notice how deleting each buffer closes their associated panes.

```vim
:bd
:bd
:ls
  1 %a   "a.txt"                        line 1
```

## Customizing and Optimizing

Beyond the fundamental commands, Vim offers a plethora of options to tailor your editing environment. By mastering buffer states (active, inactive, hidden, and unloaded), you gain a deeper understanding of Vim's text management and unlock further customization possibilities. Additionally, delve into creating and saving personalized window layouts, enabling you to craft a workspace perfectly suited to your specific tasks and preferences.

The vibrant Vim community also offers a wealth of plugins that can further enhance your buffer and window management experience. Explore these plugins to discover tools that streamline navigation, improve organization, and add even more powerful features to your arsenal.

## Embrace the Journey of Mastery

You made it! Hopefully this tutorial has demystified using buffers and panes in Vim. This barely scratches the surface of Vim's capabilities. I strongly recommend that you experiment go down rabbit holes using `:help`.

Well done, fiend. Now leave, please.
