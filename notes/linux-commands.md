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
