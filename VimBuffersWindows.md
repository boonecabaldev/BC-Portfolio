# Demystifying Vim Buffers and Windows

## Buffers
A buffer is like a variable.  It stores data but doesn't display that date. If you makes changes to a buffer and save it to a file, the data is copies from the buffer to the external file.

A window is synonymous with a "pane", which is a section within the window.  I will use the term pane in this tutorial. For the moment, we will assume you are working in one pane.  We'll cover panes more later in the tutorial.

Open vim. By default you will have one pane and one buffer.  You can view all your buffers using the `:ls` and `:buffers` commands.

You are in command mode by default.  Use `:ls`.

```vim
[output]
```
As you can see from this output, you can have one unnamed buffer stored in memory. The `:enew`command creates a new buffer which appears as a blank file. Use `:enew` followed by `:ls`.

```vim
[output]
```
You now have two buffers.

The following commands allow you to navigate between buffers.

- `:bnext`: move to next buffer (changing active buffer.)
- `:bprev`: move to previous buffer.
-  `:buffer N` or `:b N`: move to buffer with specified buffer ID.

> Each time you navigate to a buffer, that buffer becomes the "active" buffer, and the previously opened buffer becomes a hidden buffer.

Test these commands. Use `:buffer 1` to navigate to the first buffer, and then use `:buffer 2` to navigate to the next buffer.

Use `:ls` and look at the output. Notice how each buffer has an ID, name, and [research].

```vim
[id][name][...]
```
Both buffers are unnamed.
**Active buffer: 1**
Make some changes to the active buffer (1). You can save your changes to `a.txt` using the `:w a.txt` command. Use `:ls`.

```vim
[output]
```
Buffer 1 is now named `a.txt`.
```text
Mine - Confirm All

You can name a buffer by saving buffer to file using [research]
You can also name a buffer using [research].
```
```vim
[output]
```

Now move to the second buffer using `:buffer 2`. Give it a name using [name it]. List the buffers with `:ls` and notice how both buffers have names.
```
[ls]
```
You can delete a buffer using the `:bdelete` command. Navigate to buffer 2 using `:buffer 2`, and then delete it using `:bdelete`.  List the buffers with `:ls`.
```vim
[ls]
```
Now you only have one buffer.

If you have unsaved changes to a buffer and try to delete it using `:bdelete`, vim will prompt you with [research]. If you want to discard changes, use `:bdelete!`.

**Confirm**
You can also delete the active buffer using the `:q` and `:x` commands.

- `:q`: XXX
- `:x`: XXX
- `:wq`: XXX

Create a new buffer using `:enew`, type "`Buffer b`", and save the buffer to a file `b.txt` using `w b.txt`.  List buffers with `:ls`.

```vim
[ls]
```
You have named buffers for both files. 
**Active buffer: 2**
You can save and quit/delete/close buffer 2 using the convenient command `:wq`.  List buffers with `:ls`.

```vim
[ls]
```
Only buffer one is open and active. Open `b.txt` using the `:edit b.txt` command followed by `:ls`.

```vim
[ls]
```
Now we want to delete all buffers, thereby returning vim to its default state of one unnamed buffer using `:XXX`.

```vim
[ls]
```
**Confirm**
As you can see, we are back to square one.

### Buffer States
Here is an aspect of vim that illustrates its complexity.  It took a while for me to fathom the six buffer states. I will do my best to explain them.

#### Unlisted
An unlisted buffer does not appear in the list produced using `:ls` and `:buffers`. 

When would you want to use an unlisted buffer? When you want an area to manipulate temporary, expendable text. The following code creates an unlisted buffer:

```vim
" Create listed buffer
:enew 
" Make active buffer unlisted
:setlocal nobuflisted
```
Remember that you can view all buffers, both listed an unlisted, using the `:ls!`, `:ls u`, and `:buffers!` commands.
An unlisted buffer does not appear when using the `:ls` and `:buffers` commands. Look at the following example:

```vim
" Create unlisted buffer
:enew
" List all listed and unlisted buffers
:ls!
```
#### Inactive
An inactive buffer is not the active buffer, nor is it displayed in any pane. Using the `:new` command creates a new pane and buffer, thereby making the previously active buffer inactive.
#### Active
We have already described the active buffer as being buffer currently displayed in the active pane.
#### Hidden
As described earlier in this tutorial, you can have multiple buffers loaded in memory, but a pane can only display one buffer at a time. Any existing buffer not displayed in a pane is hidden. You can make a buffer hidden with the `:hide` command.
#### Loaded
Loading a buffer with content places it in a loaded state. For example, if you open a file using `:edit a.txt`, that buffer is loaded.
#### Unloaded
An unloaded buffer is one that has been removed from memory. You can use the `:bunload N` command to do this.

### Buffer State Examples
```vim
[examples]
```
## Windows
Recall that a pane (window) can one buffer at a time with each pane displaying a different buffer. Additionally, you can the same buffer open in multiple panes, allowing you to different parts of the same file.

Close and reopen vim. You have one pane with one unnamed buffer. Use the `:new` command to create a new pane. You now have two panes. Use `:ls` to view your buffers.

```vim
[buffers]
```
Notice how [explain]. I need to amend something I said earlier about active buffers. You can only have one active buffer open _per pane_. If you have multiple panes open, whichever pane contains the cursor is the active pane.

After you create a new pane with `:new`, the cursor is in the new pane. This is the active pane, and its buffer is the active buffer.

From this new pane, you can navigate all your buffers using `:bnext`, `:bprev`, and `:buffer N`, and you can open a file into the active buffer using `:edit file`. Similarly, you can navigate panes--or move the cursor--using the following commands:

- `:wincmd j`
- `:wincmd XXX`
- `:wincmd XXX`
- `:wincmd XXX`
-  `:Ctrl-w h/j/k/l`
- 
Close the active pane using `:close`. Now you are back to having one pane with one unnamed buffer. Confirm with `:ls`.

```vim
[ls]
```
Similar to buffers, you can use `:x`, `:bd`, `:q`, and `:wq` to close a window. So you can see that closing windows affects the active buffer in ways that you ought to understand.

### Challenging Example - Multiple Buffers and Windows
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
**Confirm**
### Window Settings
There are useful settings that control how window creation and splitting work with the `:new`, `:split`, and `:vsplit` commands.

- `XXX`
- `XXX`



**How to understand how closing panes affects buffers**

It's important to understand that both buffers and windows have IDs, and you use them to navigate. Understand the distinction between navigating and deleting/closing windows versus buffers; this will help you avoid getting lost when you have a lot of panes and buffers open.
