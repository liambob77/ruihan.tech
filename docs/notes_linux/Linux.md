# Linux Notes

The normal settings for /tmp are 1777, which ls shows as drwxrwxrwt.
That is: wide open, except that only the owner of a file can remove it (that's what this extra t bit means for a directory).


## Nice commands

find duplicate files, generate rm command to remove old same size file
to make it remove duplicates, remove echo from xargs.

```bash
for FILE in *; do stat -c"%s/%n" "$FILE"; done | awk -F/ '{if ($1 in a)print $2; else a[$1]=1}' | xargs echo rm
```

## shell programing tips

set -x mode (set -o xtrace), enable xtrace for shell script

- print everything as if it were executed, after substitution and expansion is applied
- indicate the depth-level of the subshell (by default by prefixing a + (plus) sign to the displayed command)
- indicate the recognized words after word splitting by marking them like 'x y'
- in shell version 4.1, this debug output can be printed to a configurable file descriptor, rather than sdtout by setting the BASH_XTRACEFD variable.

set -v mode (set -o verbose)
- print commands to be executed to stderr as if they were read from input (script file or keyboard)
- print everything before any ( substitution and expansion, …) is applied

### Making xtrace print more

xtrace output would be more useful if it contained source file and line number. Add this assignment PS4 at the beginning of your script to enable the inclusion of that information:

```bash
export PS4='+(${BASH_SOURCE}:${LINENO}): ${FUNCNAME[0]:+${FUNCNAME[0]}(): }'
```

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

Hint: These modes can be entered when calling Bash:

- from commandline: bash -vx ./myscript 
- from shebang (OS dependant): #!/bin/bash -vx 

### Inject debugging code

Insert echos everywhere you can, and print to stderr:

```bash
echo "DEBUG: current i=$i" >&2
```

If you read input from anywhere, such as a file or command substitution, print the debug output with literal quotes, to see leading and trailing spaces!

```bash
pid=$(< fooservice.pid)
echo "DEBUG: read from file: pid=\"$pid\"" >&2
```

Bash's printf command has the %q format, which is handy for verifying whether strings are what they appear to be.

```bash
foo=$(< inputfile)
printf "DEBUG: foo is |%q|\n" "$foo" >&2
# exposes whitespace (such as CRs, see below) and non-printing characters
```

## awk example

split and print

```bash
awk '{FS=","} {print $6}'
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

## coredump file check simple example

to check where coredump file will be generate

```bash
cat /proc/sys/kernel/core_pattern

```

```bash
readelf -h core.21437 
objdump86 -x core.21437 
objdump -x core.21437 
gdb test core.21437 
```

## grep usage

To search the string in a single file,

> grep file1.txt -e ‘techieshouts’

To search the full word and not the substring,

> grep -w file1.txt -e ‘techieshouts’

To search the full word by ignoring case sensitiveness,

> grep -wi file1.txt -e ‘techieshouts’

To search all the files recursively in the directory for the same word,

> grep -wir /directory -e ‘techieshouts’

To search and get the line number of the word in all the files

> grep -wirn /directory -e ‘techieshouts’

To search and get only the file names that contain the word

> grep -wirl /directory -e ‘techieshouts’

## hostname

To view the current hostname

> hostnamectl

To change the system hostname

> hostnamectl set-hostname lin

## Check the Runlevel In Linux (SysV init)

To check the system runlevel

> runlevel
> N 3

In the above output, the letter 'N' indicates that the runlevel has not been changed since the system was booted.
And, 3 is the current runlevel i.e the system is in CLI mode.


