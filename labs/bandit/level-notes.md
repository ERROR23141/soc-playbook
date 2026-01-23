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

## Level 8 → 9

- **Goal:**  
  From `data.txt`, find the only line that appears exactly once and use it as the password.

- **Commands I used:**
  - `ls`
  - `wc -l data.txt`    # checked how many lines were in the file
  - `sort data.txt | uniq -u`

- **Lessons learned:**
  - `sort` arranges lines so duplicates are next to each other.
  - `uniq -u` shows only lines that appear exactly once.
  - Piping commands together (`sort data.txt | uniq -u`) is powerful for quickly analyzing large text files.

## Level 9 → 10

- **Goal:**
  Find the password in `data.txt`. It is one of the few (human readable) strings and starts with several `=` charecters.

- **Commands I used:**
  - `ls`
  - `file data.txt`
  - `strings data.txt | head`
  - `strings data.txt | grep "==="`

- **Lessons learned:**
  - `strings <file>` extracts printable (human readable) string from a binary/mixed file.
  - You can pipe `strings` into `grep` to quickly search for patterns:
    - `strings data.txt | grep "==="`

## Level 10 → 11

- **Goal:**
  The password for the next level in stored in `data.txt` which is base64 encoded.

- **Commands I used:**
  - `ls`
  - `cat data.txt`
  - `base64 -d data.txt`
  - `cat data.txt | base64 -d`

- **Lessons learned:**
  - `base64 -d data.txt` decodes base64 encoded data.
  - You can decode a file directly `base64 -d data.txt` or use pipes `cat data.txt | base64 -d`.

## Level 11 → 12

- **Goal:**
  The password is in `data.txt` and has been encrypted with ROT13.

- **Commands I used:**
  - `ls`
  - `cat data.txt`
  - `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'`

- **Lessons learned:**
  - ROT13 rotates letters by 13 positions.
  - You can decode ROT13 with:
    - `tr 'A-Za-z' 'N-ZA-Mn-za-m'`

## Level 12 → 13

- **Goal:**
  `data.txt` is a hexdump of a file that has been compressed multiple times. Rebuild the original file and read the password.

- **Commands I used:**
  - `cd /tmp && mkdir bandit12_lab && cd bandit12_lab`
  - `cp /home/bandit12/data.txt .`
  - `xxd -r data.txt data.bin`          # hexdump → binary
  - `file data.bin`                     # see compression type
  - Repeated this pattern as needed:
    - rename: `mv data.bin data.gz` / `mv data data.bz2` / `mv data data.tar` …
    - decompress: `gunzip data.gz` / `bzip2 -d data.bz2` / `tar xf data.tar`
    - check again with: `file data*`
  - Once `file` showed **ASCII text** (`data8`):
    - `cat data8`                       # password

- **Lessons learned:**
  - `xxd -r` reverses a hexdump back into the original binary file.
  - `file` tells you what kind of compression/wrapper you’re dealing with.
  - Multi-layer compressed files can be peeled like an onion: `gzip → bzip2 → tar → …`
  - Doing all of this in `/tmp` keeps your home directory clean and safe.

## Level 13 → 14

- **Goal:**
  Use the private SSH key provided in the `bandit13` home directory to log in as `bandit14` and read the next password from 
  `/etc/bandit_pass/bandit14`.

- **Commands I used:**
  - From my machine:
    - `ssh bandit13@bandit.labs.overthewire.org -p 2220`
  - On the Bandit server as `bandit13`:
    - `ls`  
      *(saw the file `sshkey.private` in the home directory)*  
    - `ssh -i sshkey.private bandit14@localhost -p 2220`  
      *(use the private key instead of a password to log in as `bandit14` on the same box)*  
  - On the Bandit server as `bandit14`:
    - `cat /etc/bandit_pass/bandit14`

- **Lessons learned:**
  - You can log into another user on the *same* machine using `localhost`:
    - `ssh -i <keyfile> otheruser@localhost -p 2220`
  - The `-i` option tells SSH which **private key** to use for authentication.
  - When a password login is blocked, key-based auth can still be allowed.
  - Once you are the right user (here, `bandit14`), you can read files that only that user is allowed to access, like  
    `/etc/bandit_pass/bandit14`.

## Level 14 → 15

- **Goal:**
  Send the `bandit14` password to a service listening on `localhost` port `30000` and get the password for `bandit15`.

- **Commands I used:**
  - `nc localhost 30000`
  # pasted the bandit14 password and pressed Enter

- **Lessons learned:**
  - `nc` (netcat) lets you connect to a TCP port and type/paste data directly.
  - Some services simply expect a secret on a specific port and then return a response (like the next password).

## Level 15 → 16

- **Goal:**
  Use **SSL** to send the `bandit15` password to a service on `localhost` port `30001` and get the password for `bandit16`.

- **Commands I used:**
  - `openssl s_client -connect localhost:30001`

- **Lessons learned:**
  - `openssl s_client -connect host:port` opens an encrypted (TLS/SSL) connection to a service.
  - After the TLS handshake you can type into the session just like with `nc`.
  - Some services are the same idea as Level 14 but wrapped in encryption (common in real world protocols like HTTPS).


