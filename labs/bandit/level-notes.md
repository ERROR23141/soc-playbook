# Bandit Level Notes

## Level 0 -> 1
- **Goal:**  
  Get the password for the next level (bandit1) by reading the `readme` file on the server.

- **Commands I used:**  
  - `ssh bandit0@bandit.labs.overthewire.org -p 2220`  
  - `ls`  
  - `cat readme`  

- **Lesson learned:**  
  - How to SSH into a remote Linux box.  
  - How to list files and read a file from the terminal.

## Level 1 → 2
- **Goal:**
  Get the password for the next level (bandit2) by reading the file named `-` on the server.

- **Commands I used:**
  - `ssh bandit1@bandit.labs.overthewire.org -p 2220`
  - `ls`
  - `cat ./-`
  - `cat < -`

- **Lesson learned:**
  - Files that start with `-` can be treated like options by commands.
  - You can read them by:
    - Using a relative path like `./-`
    - Or using input redirection like `cat < -`

## Level 2 → 3
- **Goal:**
  Read the contents of a file whose name contains spaces and starts with `--` to get the next password.

- **Commands I used:**
  - `ls`
  - `cat -- "--spaces in this filename--"`
  - `cat "./--spaces in this filename--"`

- **Lesson learned:**
  - Filenames can be tricky when they:
    - Start with `-` (they look like options), or contain spaces.
  - You can handle them by:
    - Using `--` to mark the end of options: `cat -- <filename>`
    - Or prefixing with `./` and quoting the name: `cat "./filename with spaces"`



