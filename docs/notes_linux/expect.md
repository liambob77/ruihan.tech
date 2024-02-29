
## expect tcl

Example handle first time ssh login



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

