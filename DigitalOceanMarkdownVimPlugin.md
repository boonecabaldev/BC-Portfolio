
# How to Create a DigitalOcean Markdown Vim Plugin
This tutorial is for the intermediate-level vim user who wants to expand his or her knowledge and skill with vim.

The basics of vim are great, but they will only get you so far. The real power of vim lies in its potential for customization via its scripting language, VimL.  This tutorial will walk you through creating a plugin that runs a Linux command--in this case, python3--and displays the output in a pane docked at the bottom of the window.   The goal is to show you how this is done under the hood, this way you can use this knowledge to craft your own plugins.  When you are done with this tutorial, you will have experience with the following:

## Prerequisites
- A working understanding of how to operate a Linux command line terminal.  The DigitalOcean tutorial [A Linux Command Line Primer](https://www.digitalocean.com/community/tutorials/a-linux-command-line-primer) is a great place to start.
- An intermediate skill level using Vim. This tutorial builds on the skills you read about in[How To Use Vim for Advanced Editing of Plain Text or Code on a VPS](https://www.digitalocean.com/community/tutorials/how-to-use-vim-for-advanced-editing-of-plain-text-or-code-on-a-vps-2).

## Setting Everything Up

Let’s begin by installing vim and setting up the appropriate directories and files. Enter the following commands to install vim:

```command
sudo apt-get update
sudo apt-get install vim-gtk3
```

Your main vim configuration file is called `.vimrc`.  If it doesn't already exist, create one in your home directory using the following command:

```command
touch ~/.vimrc
```

Vim uses a special `~/.vim` directory to store additional files like your plugin. Use the following command to create it as well as the plugin sub-directory:

```sh
mkdir -p ~/.vim/plugin
```

<$>[note]
**Note:** The `-p` argument to `mkdir` creates both the parent as well as the child folder even if the parent folder doesn't exist.
<$>

<$>[note]
**Note:** The `~/.vim` folder likely won't exist after a fresh install of vim.
<$>

Create your plugin file using the following commands:

```command
cd !$
touch ~/.vim/plugin/run_command.vim
```

<$>[note]
**Note:** The `!$` keyword represents the last directory created.
<$>

You need to modify your `.vimrc` file.  Open it using the following command:

```sh
vim ~/.vimrc
```

Link your plugin file to the plugin file using the `source` command. Go to the end of the file and add the following lines:

```vim
source /workspaces/blank-codespace/.vim/plugin/run_command.vim
```
<$>[note]
**Note:** `/workspaces/blank-codespace` is the default directory in a fresh, blank GitHub codespace.
<$>
Save and exit using the `x` command.

## How It Works
The best way to understand how this plugin works is the walk through its main use cases, starting with the basic success path. First, you will a python file using the following bash command:

```sh
vim test.py
```

This is the initial screen:

[initial-screen]

Run the plugin using the following vim command:

```vim
:call RunCommand('python3')
```

Here is the screen with the output of the python script in a pane docked at the bottom of the window:

[output-screen]

Now, let's say the vim window is split into multiple panes and buffers before you run the python script, as below:

[multple-panes-open]

As you can see, there are multiple panes open.  Therefore, your plugin will close every pane and buffer first, and then it will execute the python script.  For this reason your plugin will expose two functions: `ClearAllBuffers` and `RunCommand`.

## Step One - Creating the Scaffolding Code

It is time to start building your plugin.  Open the plugin file in vim with the following command:

```command
vim ~/.vim/plugin/run_command.vim
```
Your vim plugin is comprised of two functions: `ClearAllBuffers` and `RunCommand`. Create the following scaffolding code to your plugin:

```vim
function! ClearAllBuffers()
    echo "In ClearAllBuffers()"
endfunction

function! RunCommand(command)
    echo "In RunCommand()"
endfunction
```

Every time you save your plugin, use the command-line commands `w`, then `so %`.  Doing so will source the current changes in the file.  Otherwise, you would have to save and then reopen the file.

<$>[note]
**Note:** `%` is a special vim command that refers to the current file.
<$>

Test the scaffolding code using `call RunCommand('python3')`. You should see the message "`In RunCommand`" in the vim output:

```
[label Output]

In RunCommand
```
Good. Your scaffolding code is set up.

## Step Two - Making `ClearAllBuffers` Function

Let's create the `ClearAllBuffers` function first, which does the following:
1. Closes all window panes except the current one.
2. Clears all buffers except the current one.

<$>[note
Remember: Every time you run a python script, you first reset vim such that there is only one buffer and no panes open.
<$>
The first thing you need to do is save the currently opened file name stored in the `@%` buffer.  Later in the code you will lose access to this.

YYYYYYYYYYYYYYYYYY

```vim
function! ClearAllBuffers()

    " Save current filename, as it will be lost after clearing all buffers.
    let temp_filename = @%

endfunction
```

<$>[note]
**Note:** The `@%` buffer stores the open file name.
<$>

Save the current file using  `silent w`, which suppresses vim output. If there is too much output, you will have to press the `Enter` key to clear it every time you run `ClearAllBuffers`.

```vim
function! ClearAllBuffers()

    let temp_filename = @%

    " Save current filename.
    silent w

endfunction
```

Close all open buffers using `%bd!`.  After clearing all buffers, `@%` won't have the current file because all the buffers are closed, and the active buffer has reset.  To reiterate, this is why you saved the filename in a temporary variable.

```vim
function! ClearAllBuffers()

    let temp_filename = @%
    silent w

    " Close all buffers.
    %bd!

endfunction
```
<$>[note]
**Note:** `%` applies the command to all lines in the buffer, and the `!` forces the command without any prompts.
<$>

Read the previously opened file into the current buffer using `execute "read " . temp_filename`.  You are using `execute` to construct a command string to run.

```vim
function! ClearAllBuffers()

    let temp_filename = @%
    silent w
    %bd!

    " Read the file back into the current buffer.
    execute "read " . temp_filename

endfunction
```

Move the cursor up one line with normal-mode `k`, then delete the line under the cursor with `dd`. When you read the contents of the file, there is a blank line above the content and cursor; therefore, you are moving the cursor up and deleting it.

```vim
function! ClearAllBuffers()

    let temp_filename = @%
    silent w
    %bd!
    execute "read " . temp_filename

    " After you open python file using read it will prepend a blank line at the op.  Delete it.
    normal kdd

endfunction
```

Save the open file the last time again using `silent execute "w! " . temp_filename`.

```vim
function! ClearAllBuffers()

    let temp_filename = @%
    silent w
    %bd!
    execute "read " . temp_filename
    normal kdd

    " Save the file.
    silent execute "w! " . temp_filename

endfunction
```

Optionally print a success message.

```vim
function! ClearAllBuffers()

    let temp_filename = @%
    silent w
    %bd!
    execute "read " . temp_filename
    normal kdd
    silent execute "w! " . temp_filename

    echo "Well done, friend.  Now leave, please"

endfunction
```

## Step Three -- Testing the `ClearAllBuffers` Function

Let's test `ClearAllBuffers`.  Create multiple panes and buffers using the `sp` command two times.  The window should look like the following:

[three-panes]

Call the function using `call ClearAllBuffers()`.  All the panes should be gone.  Additionally, confirm all the buffers are closed using `buffers`.  There should only be one buffer.

## Step Four -- Creating the `RunCommand` Function

Let's start coding `RunCommand`.  First, clear all buffers and close all window panes using `ClearAllBuffers`.  As mentioned in the _How It Works_ section, you close all buffers and panes first because you could potentially have multiple panes open already.

```vim
function! RunCommand(command)

    call ClearAllBuffers()

endfunction
```

Save the open python file name into a temporary variable.


```vim
function! RunCommand(command)

    " Save the open file name
    let temp_filename = @%

endfunction
```

Create a pane using `sp` command.  At this point the cursor will be in the top pane; we want it in bottom pane. After executing `sp` the bottom pane will be a duplicate of the top pane, and both will have the python file opened. 

```vim
function! RunCommand(command)

    let temp_filename = @%
    call ClearAllBuffers()

    " Create bottom pane for python output. This leaves cursor in top pane. We
    " want cursor to be in bottom pane.
    sp

endfunction
```

Move the cursor to the bottom pane using `wincmd j`.

```vim
function! RunCommand(command)

    let temp_filename = @%
    call ClearAllBuffers()
    sp

    " Move cursor to bottom pane.
    wincmd j

endfunction
```

Create a fresh buffer in the bottom pane using `enew`. 

```vim
function! RunCommand(command)

    let temp_filename = @%
    call ClearAllBuffers()
    sp
    wincmd j

    " Create a fresh buffer in the bottom pane.
    enew

endfunction
```

Execute the python file and read its output it into the bottom buffer.  Notice you are using the temporary filename `temp_filename` you saved earlier.

```vim
function! RunCommand(command)

    let temp_filename = @%
    call ClearAllBuffers()
    sp
    wincmd j
    enew

    " Execute the python file and read it into the bottom 
    " buffer.
    silent execute "!" . a:command . " . temp_filename

endfunction
```

Again there is a blank line above the python output.  Delete it again using `normal kdd`.

```vim
function! RunCommand(command)

    let temp_filename = @%
    call ClearAllBuffers()
    sp
    wincmd j
    enew
    silent execute "!" . a:command . " . temp_filename

    " Delete the blank line above the python output.
    normal kdd

endfunction
```

Finally, move the cursor back up to the top pane using the `wincmd p`.  Your python file will be opened there.

```vim
function! RunCommand(command)

    let temp_filename = @%
    call ClearAllBuffers()
    sp
    wincmd j
    enew
    silent execute "!" . a:command . " . temp_filename
    normal kdd

    " Move cursor back up to top pane
    wincmd p

endfunction
```

You are done.  Save the file and close it with `x`.

## Step Five -- Testing the `RunCommand` Function

Time to test `RunCommand`. Open a new `test.py` python file the following terminal command:

```command
vim vim test.py
```

Enter the following content into the file.

```python
print("Well done, friend. Now leave, please.")
```

Save the file with `w` and use `call RunCommand('python3')`.

You should see the following output.

```
[secondary_label Output]
Well done, friend. Now leave, please.
```

Try opening multiple panes using `sp` and then run `RunCommand`.  The result will be the same.

## Step Five - Creating Maps

YYYYYYYYYYYYY  is mapleader global?

You don't want to enter command-line mode and use `call RunCommand('python3')` every time.  Instead, you will create a map for `ClearAllBuffers` and `RunCommand`.  Add the following code to the bottom of the file. You can use any key combination. I happen to use these.  Don't forget to test them.

```vim
nnoremap <leader>b :call ClearAllBuffers()<CR>
nnoremap <leader>r :call RunCommand('python3')<CR>
```

I like to define a `<leader>` key.  I use the `';'` character.  Enter the following code above your maps.

```vim
let mapleader = ";"
```

Congratulations!  You are done making your vim plugin!

## Conclusion

You learned a lot in this tutorial, but this is just the beginning.  Hopefully this tutorial has created enough momentum for you to create your own plugins. I hope this tutorial helped you.  Have a very pleasurable evening.  Now leave, please.

## Complete Code

Here is the complete code for the the `run_python.vim` plugin:

```vim
"
" Function removes all buffers except the active one.
"
function ClearAllBuffers()
  " Save current filename, as it will be lost after clearing all buffers.
  let temp_filename = @%

  " Save current python file.
  silent w

  " Clear all buffers.  You will lose the current file.
  %bd!

  " Open the current python file
  execute "read " . temp_filename

  " After you open the file using :read, it will prepend a blank line at
  " the top.  We delete it here.
  normal kdd

  " Save the python file.
  silent execute "w! " . temp_filename

  " Give message saying we're good to go.
  echo "Ready to edit " . temp_filename . ", fiend."

endfunction

"
" Clears all buffers, runs python3 on current file, and sends output to
" bottom pane.
"
function RunCommand(command)

  call ClearAllBuffers(0)

  " Save current python file.
  let temp_filename = @%

  " Create bottom pane for python output. This leaves cursor in top pane.
  " We want the cursor to be in the bottom pane at this juncture.
  sp

  " Moves cursor back to top pane.
  wincmd j

  " Erases current buffer.  Current python file is opened in both top and
  " bottom pane.  We only want the python file open in the top pane.
  enew

  " Run python file, sending output to bottom pane.
  silent execute "!" . a:command . " . temp_filename

  " After you open the file using :read, it will prepend a blank line at the
  " top.  We delete it here.
  normal kdd

  " Move cursor back up to top pane.
  wincmd p

endfunction

let mapleader = ";"

nnoremap <leader>b :call ClearAllBuffers()<CR>
nnoremap <leader>r :call RunCommand('python3')<CR>
```
