# Vim Buffers and Windows

## Buffers

A buffer is an area in memory that stores text. When you open a file, its content is read into a buffer.  When you makes changes to that buffer and save it to a file, vim copies the buffer to a file on the hard drive.

A buffer doesn't display the text, a window, or "pane",  does.

> This tutorial will use the term pane instead of window. A

A pane is is a section within the window. A pane can only display one buffer at a time. For simplicity, we'll assume we're working in a single pane.  We'll cover panes in more detail later.

### Viewing and Creating Buffers

Let's begin by opening vim. By default you will have one pane and one buffer.  You can view all your buffers using `:ls` or `:buffers`.

While in command mode, use `:ls`.

```vim
:ls
  1 %a   "[No Name]"                    line 1
```

The `:ls` command produces output with the following format:

```text
Number Indicator    Name                          Line
```

```vim
     1        %a   "[No Name]"                    line 1
```

All of these fields are self-explanitory except for Indicator. Here is an excerpt from the vim help for `:ls`:

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

The Indicator in the above `:ls` output is `%a`, which specifies that the buffer is active (`a`) and in the current window or pane (`%`).

### Creating Buffers

Before we can create a new buffer, we need to save the active buffer to a file, effectively giving it a name. You can do this using `:w a.txt`. Add the text `File a.txt` to the buffer followed by the commands `:w a.txt | ls`, producing the following output:

> `:w` is the equivalent of the typical Save command in Windows.

```vim
"a.txt" [New] 1L, 11B written
  1 %a   "a.txt"                        line 1
```

> You can use the `|` operator to invoke multiple command-mode commands.

Notice that the buffer is now named `a.txt` and it has a number of `1`. Now you can create a new buffer.

### Saving Buffers to Files

You can create a new, unnamed buffer using `:enew`. Use the commands `:enew | ls`, which produces the following output:

```vim
:enew | ls
  1 #    "a.txt"                        line 1
  4 %a   "[No Name]"                    line 1
```

Now we two buffers

- buffer `1`: `a.txt`
- buffer `4`: `[No Name]`, the active buffer

### Deleting Buffers

You can delete a buffer using `:bdelete`. Try to delete the active buffer, buffer 2 using the commands `:bdelete | ls`:

```vim
"a.txt" 1L, 11B
  1 %a   "a.txt"                        line 1
```

We deleted buffer 4, and now we're back to just buffer 1. If you had made any changes to buffer 4 before deleting it, vim would have displayed a message saying you have unsaved changes to the active buffer. You can `:bdelete` to skip this message and dicard changes.

### Exiting Vim and Deleting Buffers

You can exit vim using the following commands:

- `:q`: Quit the current window, which can be either the pane or the Vim window. Fails when changes have been made.
- `:wq`: Write current file and close window. Quit if last edit. Writing fails when buffer is unnamed.
- `:x`: Like `:wq`, except write ony when changes have been made.

If you want to delete all open buffers without quitng Vim, you can use `:bd` or `:bd!`. You should be at a bash prompt. Create a new file `b.txt` using the following command:

```bash
cat << EOF > b.txt
File b.txt
EOF
```

Open `a.txt` and `b.txt` in Vim using the following command:

```bash
vim a.txt b.txt
```

You should have both files open.

```vim
:ls
  1 %a   "a.txt"                        line 1
  2      "b.txt"                        line 0
```

Vim has a great command `:bufdo [command]` that allows you to perform a command over each buffer. Using `:bufdo bd` followed by `:ls` produces the output:

```vim
:ls
  3 %a   "[No Name]"                    line 1
```

### Opening Files

You can use the `:e` or `:edit` command to open a file. Use `:e a.txt` to open file `a.txt`, followed by `:ls`:

```vim
"a.txt" 1L, 11B
  1 %a   "a.txt"                        line 1
```

Open `b.txt` using `:e b.txt | ls`.

```vim
"b.txt" 1L, 11B
  1 #    "a.txt"                        line 1
  2 %a   "b.txt"                        line 1
```

Now that we have two named buffers open, let's learn how to navigate between buffers.

### Navigating Beteen Buffers

The following commands allow you to navigate between buffers:

- `:[N]bnext [N]` or `:[N]bn [N]`: move to next buffer, where N is the buffer number.
- `:[N]bprev [N]` or `:[N]bp [N]`: move to previous buffer, where N is the buffer number.
- `:[N]buffer [N]` or `:[N]b [N]`: move to specified buffer N, where N is the buffer number.

From the previous `:ls` command, we have buffers 1 and 2 open, with 2 being the active buffer.

### Buffer States

Buffer states are complex; it took me a while to understand them. Let's cover each of the six buffer states.

#### Unlisted

An unlisted buffer does not appear in the list produced using `:ls` or `:buffers`.

When would could you use an unlisted buffer? Whenever you want to temporarily create or edit text. Once you're done, you can delete the buffer and discard the changes. The following code creates an unlisted buffer:

```vim
" Create listed buffer
:enew 
" Make active buffer unlisted
:setlocal nobuflisted
```

Understand: `:ls!`, `:ls u`, and `:buffers!` display all buffers, both listed and unlisted. Conversely, `:ls` and `:buffers` _do not_ display unlisted buffers. The following code illustrates this:

```vim
" Create unlisted buffer
:enew
" List all listed and unlisted buffers
:ls!
```

#### Inactive

An inactive buffer is one that is not displayed in any pane. Using the `:new` command creates a new pane and buffer, thereby making the previously active buffer inactive.

#### Active

The active buffer is the one that is currently displayed in _any_ active pane.

#### Hidden

You can have multiple buffers loaded in memory, but a pane can only display one buffer.

> All your buffers are in memory, and one buffer is displayed in the active pane.

 Any buffer not displayed in _any_ pane is hidden. You can make a buffer hidden using `:hide`.

#### Loaded

Editing a file places its buffer in a loaded state. For example, if you open a file using `:edit a.txt`, that buffer is loaded.

#### Unloaded

An unloaded buffer is one that has been removed from memory; `:bunload N` does this.

#### Buffer State Examples

```vim
[examples]
```

## Panes

You can create multiple panes, allowing you to edit different buffers. Alternatively, you can edit the same buffer in multiple panes, allowing you to view and edit multiple parts of the same buffer.

Similar to how you can only have one active buffer, you can only have one active pane, and each pane has its own active buffer. Therefore, you can have multiple active buffers. Whichever pane contains the cursor is the active pane.

> You may need to reread this a few times to understand.

### Creating Panes

There are useful settings that control how window creation and splitting work with the `:new`, `:split`, and `:vsplit` commands.

- `:new`
- `:split`
- `:vsplit`

Close and re-open vim. You have one pane and one unnamed buffer. Use `:new` to create a second pane and buffer, and then `:ls` to view buffers.

```vim
[buffers]
```

Notice how the new pane contains the cursor; this is the active pane, and its fresh, unnamed buffer is the active buffer.

**DEMONSTRATE `:split`**

**DEMONSTRATE `:vsplit`**

### Pane Settings

There are settings that control the behavior of `:split` and `:vsplit`.

XXX

### Navigating Between Panes

You can navigate between panes using `:bnext`, `:bprev`, and `:buffer N`. Understand that navigating between panes simply means moving the cursor from one pane to another, thus activating a new one.

- `:wincmd j`
- `:wincmd XXX`
- `:wincmd XXX`
- `:wincmd XXX`
- `:Ctrl-w h/j/k/l`

It's important to understand that both buffers and windows have numbers which identify them, and you use these numbers for navigation, as well as other functions. Understanding the difference between navigating between panes versus buffers will help you avoid getting lost when you have lots of open panes and buffers.

### Deleting Panes

Similar to buffers, you can use `:x`, `:bd`, `:q`, and `:wq` to close a window.

Understand that some commands delete buffers, and some delete both panes and buffers. You can delete a buffer, or a buffer and a pane, but you can't delete a pane without deleting a buffer.

Delete the active pane using `:close`. Now you have one pane and one unnamed buffer. Confirm with `:ls`.

```vim
[ls]
```

## Challenging Example - Working With Multiple Buffers and Panes

```text
- buffers a.txt, b.txt, c.txt, d.txt
- 3 windows
- pane 1: a.txt, a.txt
- pane 2: b.txt
- pane 3: c.txt
- c.txt is hidden
- delete pane 1, a.txt, b.txt
- :ls to confirm a.txt, b.txt are deleted
- state:
-   pane 2: b.txt
-   pane 3: c.txt
- open c.txt in pane 3
- delete pane 3
- state
-   pane 2 has b.txt
- bd
- init vim state
```
