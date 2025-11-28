# Bandit Level Notes

## Level 0 → 1
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

## Level 3 → 4
- **Goal:**
  Find the password stored in a *hidden* file inside the `inhere` directory.

- **Commands I used:**
  - `ls`
  - `cd inhere`
  - `ls -a`
    (showed a hidden file named `...Hiding-From-You`)
  - `cat ...Hiding-From-You`

- **Lesson Learned:**
  -  Hidden files start with a dot `.` and dont show up with normal `ls`.
  -  Use `ls -a` (or `ls -la`) to see hidden files.
  -  If `cd <name>` gives `Not a directory` it means its a **file** not a folder.

## Level 4 → 5
- **Goal:**
  Find the only human-readable file in `inhere` that contains the next password.

- **Commands I used:**
  - `ls`
  - `cd inhere`
  - `ls -a`
    (saw files named `-file00` to `-file09`)
  - `file ./*`
    (to check which file is ASCII text (human-readable))
  - `cat < -file07`

- **Lesson learned**
  - The `file` command is useful to quickly identify which files are text or binary.
  - Filenames that start with `-` are treated like options by many commands.
  - You can read those files using:
    - `cat -- -file07`
    - `cat ./-file07`
    - `cat < -file07` (input redirection)

## Level 5 → 6
- **Goal:**
  Find the only file in `inhere` that matches specific properties (size and permissions) and get the password from it.

- **Commands I used:**
  - `ls`
  - `cd inhere`
  - `ls -a`
    (saw dir named 'maybehere00`...`maybehere19`)
  - `find . -type f -size 1033c ! -executable`
  - `cd maybehere07`
  - `cat .file2`

- **Lessons learned:**
  - `find` can search with filters
    - `-type f` → only files
    - `-size 1033c` → size is exactly 1033 bytes (`c` = bytes)
    - `! -executable` → exclude executable files

## Level 6 → 7
- **Goal:**
  Find a file on the system that is:
  - owned by user `bandit7`
  - owend by group `bandit6`
  - 33 bytes in size
  and read its contents to get the next password.

- **Commands I used:**
  - `cd /`
  - `find . -user bandit7 -group bandit6 -size 33c 2>/dev/null`
  - `cat ./var/lib/dpkg/info/bandit7.password`

- **Lessons learned**
  - `find` can search by:
    - `-user <name>` → file owner
    - `-group <name>` → group
    - `-size 33c` → file size
  - `2>/dev/null` hides "Permission denied" errors when searching from `/`.

## Level 7 → 8
- **Goal:**
  Find the line in `data.txt` that contains the word `millionth` and use the value next to it as the password.

- **Commands I used**
  - `ls`
    (showed data.txt file which contains a long list of txt)
  - `grep millionth data.txt`

- **Lessons learned**
- `grep <pattern> <file>` searches for lines in a file that match a pattern.
  (eliminates the need to scroll through the file manually)
- `grep` is a key tool for log analysis and text processing.

