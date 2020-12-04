# Netcat (nc) command examples
Here’s the syntax for the nc command:

``` bash
nc [options] [hostname] [port]
```

The syntax can vary depending on the application, but for most uses, your commands will follow this basic pattern.

## 1. Create a Connection Using TCP with netcat command
As I mentioned earlier, the core functionality of netcat is joining two machines together. You can set up a connection using TCP to connect two separate machines, you can also emulate that connection using the terminal.

The Listening Machine:

``` bash
nc -l 8080
```
This command opens port 8080 and tells the machine to begin listening on this port.

In order to establish a connection, you will use another terminal and enter the following.
