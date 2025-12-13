# Linux Commands Cheatsheet

## Navigation

- `pwd` – print current working directory
- `ls` – list files in the current directory
- `ls -la` – list files including hidden files with details
- `cd` – change directory

## Files and Directories

- `cat filename` – show file contents
- `touch filename` – create an empty file or update its timestamp
- `cp src dst` – copy a file
- `mv src dst` – move or rename a file
- `rm filename` – delete a file
- `mkdir dir` – create a new directory
- `rm -r dir` – delete a directory and its contents
- `file filename` - detect file type (text, binary, gzip, bzip2, tar, etc.)
- `xxd -r in.txt out.bin` - reverse a hexdump back into a binary file

## Permissions

- `chmod 600 filename` – owner read/write only
- `chmod +x script.sh` – make file executable
- `ls -l` – see permissions and ownership

## Networking

- `ping <IP_or_domain>` – test connectivity to a host or domain
- `ip a` – show interfaces and IP addresses
- `hostname -I` – show IP addresses quickly
- `ssh user@host` – connect to a remote machine over SSH
- `curl http://host:port` – make an HTTP request from the terminal
- `nc host port` - open a raw TCP connection to a host/port (netcat)
  - Example: `nc localhost 30000`
- `openssl s_client -connect host:port` - open an SSL/TLS connection
  - Example: `openssl s_client -connect localhost:30001`

## System and Services

- `sudo` – run a command as root
- `sudo apt update && sudo apt upgrade` – update packages
- `sudo systemctl status ssh` – check status of SSH service
- `sudo systemctl restart ssh` – restart SSH service
- `sudo systemctl enable --now ssh` – enable and start ssh service

## Searching 

- `grep "text" file` – search for text in a file
- `grep -r "text" .` – search recursively in current directory
- `find . -name "pattern"` – find files by name starting from current directory

## Useful Bandit Tricks

- `cat ./-` – read a file that starts with `-`
- `cat -- "--spaces in this filename--"` – read a file that starts with `--` and has spaces

## Quick web server

- `python3 -m http.server 8080` – start a simple HTTP server in the current directory on port 8080

## Inspecting / decoding data

- `strings file` – show printable strings inside a binary or unknown file.
- `strings file | grep "pattern"` – search for a string inside a binary file.
- `base64 -d file` – decode a base64-encoded file.
- `cat file | base64 -d` – decode base64 from stdin (piped input).
- `cat file | tr 'A-Za-z' 'N-ZA-Mn-za-m'` – decode/encode ROT13.

## Compression & Archives

- `tar xf archive.tar` - extract a tar archive
- `gunzip file.gz` - decompress a gzip file
- `bzip2 -d file.bz2` - decompress a bzip2 file
