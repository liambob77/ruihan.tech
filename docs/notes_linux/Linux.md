# Linux Notes

> The normal settings for /tmp are 1777, which ls shows as drwxrwxrwt.
> That is: wide open, except that only the owner of a file can remove it (that's what this extra t bit means for a directory).

## shell programing tips

enable xtrace for shell script

- The `set -x` option instructs bash to print commands and their arguments as they are executed.
- The `set -x` option instructs bash to print shell input lines as they are read.

[Unofficial Bash Strict Mode](http://redsymbol.net/articles/unofficial-bash-strict-mode/ "Title")

- The `set -e` option instructs bash to immediately exit if any command has a non-zero exit status.
- The `set -u` option instructs bash to immediately exit if a reference to any variable you haven't previously defined - with the exceptions of $* and $@.
- The `set -o` pipefail option instructs bash to prevents errors in a pipeline from being masked.
- Setting IFS to $'\n\t' means that word splitting will happen only on newlines and tab characters.

```bash
#!/usr/bin/env bash
# Uncomment following line for Debugging
# set -xv

set -euo pipefail
IFS=$'\n\t'
export LC_ALL=C
```

## awk example

split and print

```bash
awk '{FS=","} {print $6}'

## Nice commands

find files with same size, generate rm command to remove old same size file

```bash
for FILE in *; do stat -c"%s/%n" "$FILE"; done | awk -F/ '{if ($1 in a)print $2; else a[$1]=1}' | xargs echo rm
```
```

## expect example

handle first time ssh login

```bash
expect <<EOF
set timeout 10

spawn ssh root@$TARGET_IP
expect {
    "Password" {send "$PWD\r";}
    "yes/no" {send "yes\r";exp_continue}
}
expect "#*"

send "mv $BIN $BIN.bak;\r"
send "exit\r"
expect eof

EOF
```
