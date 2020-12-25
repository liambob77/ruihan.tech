# Linux Notes

> The normal settings for /tmp are 1777, which ls shows as drwxrwxrwt.
> That is: wide open, except that only the owner of a file can remove it (that's what this extra t bit means for a directory).

## shell programing tips

- enable xtrace for shell script

```bash
set -x
set -xv
```

## awk example

- split and print

```bash
awk '{FS=","} {print $6}'
```
