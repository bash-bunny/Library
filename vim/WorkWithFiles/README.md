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
