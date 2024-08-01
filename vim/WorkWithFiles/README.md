# Read

Vim allows you to read an entire file and insert it's contents
- `:read` or `:r`
  - `:r file.txt` - insert the file *file.txt* under the cursor in the actual **buffer**
  - `:0r file.txt` - insert the *file.txt* under the first line of the file
  - `:r!sed -n 2,8p file.txt` - insert the lines from 2 to 8 from file *file.txt* under the cursor
  - `:r !ls` - insert the result of `ls` under the cursor

# Goto

Vim allows you to open other files or urls directly 

- `gf` - Opens the file or path under the cursor
- `gx` - Opens the url in the default browser

# Close Files

- `:wq` - Saves the file with the name you've given and exits vim
- `:x` - Exits vim but only saves the file if it has any change
  - `ZZ` - The same as `:x`
- `:q!` - Exits vim without saving the file
  - `ZQ` - The same as `:q!`
- `:qa` - Exits and close all open file

# Save Files

- `:w` - Save the current open file
- `:w file.txt` - Save the current file as **file.txt**, if you save again with `:w` you save the original filename
- `:w! file.txt` - Save the actual file as **file.txt** with the overwrite option
- `:sav file.txt` - Save the actual **buffer** as a new **file.txt**
- `:up[date] file.txt` - Like `:w` but only save the file when the **buffer** was modified

## Tips

- `:w /tmp/backup` - When you don't have rights to save a file
- `:w !sudo tee %` - To save a file as root

# Vim Navigation

## Basic

- With arrow keys (it works in insert mode too)
- With the letters `j`, `k`, `l`, `h` for down, up, right, left

## By word

A **Word** in vim could be
- **WORD**: A set of characters that doesn't include a blank space
- **word**: A set of characters that doesn't include special characters (like: `.,-)`)

So to move by words in vim
- `w` - goes to the start of the next **word**
- `W` - goes to the start of the next **WORD**
- `e` - goes to the end of the next **word**
- `E` - goes to the end of the next **WORD**
- `b` - goes to the previous **word**
- `B` - goes to the previous **WORD**

### Tips

1. If you work with code, it's better to use the **word** commands to stop in special characters. With plaintext it's better to use the **WORD**s commands
2. Every command accept numeric prefixes, so it repeats the command *n* times (`3w` move the cursor 3 words)
