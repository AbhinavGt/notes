# Old editor 

- ed is a line-oriented text editor originally developed for Unix in 1973.

```bash
➜  ~ ed summer.md
147

?
1
# Inheritance

```python

Class car {
s/car/bus
Class bus {
q
?
q
```

- ex is a line editor based on and intended as an improvement to ed, the original Unix editor.
	- It also comes with Bill Joy's vi mode
	- Uses ram < 1k

```bash
➜  ~ ex summer.md
"summer.md" 10L, 147B
Entering Ex mode.  Type "visual" to go to Normal mode.
:1
# Inheritance
```python
Class car {
:s/car/bus
Class bus {
:vi
# Inheritance
```python
Class bus {
    String name
    int speed = 200;
    public void f1(){
        System.out.println("I am car");
    }
}
~
~
~
~
~
:q!
```

- `Vi` is a visual mode of the `ex` text editor, which means it provides a full-screen interface for editing
- while `ex` is a line-oriented editor that operates primarily through command-line inputs. 
- Both editors are essentially the same program.
- `Vim` stands for `Vi IMproved`.
- Vim provides both a terminal screen user interface and a graphical user interface (gVim).
# Vim manual
- `vimtutor` on terminal
- `vim-adventures` on web
- `:h usr<tab>` on vim
# Terminology
- Buffer
	- A buffer contains the text of the file and is what you edit
	- It is representation of file in memory. 
	- Editing the buffer does not mean you edit the file directly.
	- When you write the buffer you are writing it to the file and replacing its content. 
	- `:h buffer`
- Windows
	- Contains a buffer to display. Windows can be closed but the underlying buffer can remain in memory.
	- `:h window`
- Tabs
	- A tabs is like another viewport. You can have many window/splits open per tab.
	- `:h tab`
- Splits
	- A split simply refers to splitting the viewport in N section (various sizing and orientations available) to display windows.
	- `:h split`
	- `:new` -> Creates a horizontal split with new buffer
	- `:vnew` -> Creates a vertical split with new buffer
## Other Terminology

![[vim-terminology.png]]

- Line Number :- Displays line numbers for navigation. Can be relative and absolute
- Cursor :- Indicates the current editing position.
- Color Column :- A vertical guide marking a preferred maximum line width that you consider is enough
- Tabs/current tab :- Shows the currently selected tab page.
- Command Line/Current Mode :- Where you type command and it also displays the mode that you are in.
- Status Line :- Shows file information, mode, position, Git branch, diagnostics, etc.
- Sign Column :- Displays Git signs, diagnostics, breakpoints, etc.
# Features
- Help Menu
	- Help menu can be accessed by typing `:h<enter>`
- Motion
	- A command that moves the cursor
	- `:h motion`
- Abbreviations
	- `Ctrl + a` will be abbreviated `<C-a>`
	- ENTER will be abbreviated `<CR>`
	- TAB `<tab>`, ESCAPE `<esc>`, SPACE `<space>`
	- `:` means that it will execute a command.
# Using Vim
`vim `

![[VIM-start.png|300]]
- This is just a buffer window. It has no line number and nothing
## Modes
- There are a few modes:
	- Normal
	- Insert
	- Visual
	- Visual Line
### Normal Mode
- This is where you can execute commands to navigate, edit, and execute vim/sys commands
- `vim test.js`
- You are now in `NORMAL` mode
### Insert Mode
 - This is where you can edit the file.
- After pressing `i` you should see something like `-- INSERT --` in the bottom left hand side of vim.

