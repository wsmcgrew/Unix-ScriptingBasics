# Shell Input Output

## 2.2.1

"Everything is a file"

File descriptors can be refrenced using three diffrent commands. Calling a function like read to an object, and th OS will take care of that lower level functionality which th OS will map

``` 
Linux       lsof 
FreeBSD     fstat
Solaris     pfiles
```
Using `lsof -p $$` (pasing a PID), it will show the Command, Pid, User, File Descriptor, File Type, Device, Size/Offset, node, and name of file.

`fstat` has similar output. It has User, Command, PID, File descriptor, WHere its mounted, Mode, and size.

For solaris, `pfiles` has alot of the same information. However instead of the neat format, all information is presented within a specific format. 

Each FD has three standard descriptors. 

| File Descriptor | Name | Default "file" |
| --- | --- | --- |
| 0 | Standard in (STDIN) | Keyboard |
| 1 | Standard out (STDOUT) | Terminal Display |
| 2 | Standard Error(STDERR) | Terminal Display | 

File descriptors can be redirected into different areas, other than the terminal and keyboard. 

```
    - <   Files contents
    - >   Redirect to a processes location (commonly sent to a file.)     It will overwrite
    - >>  Will append to an existing file.
    - n>  allows you to redirect to a specific output.
```
and example, say we create a file called input_file with `alpha, bravo, charlie, delta`
Using `grep a < input_file` it will look to the file for strings that contain the letter "A"

Piping commands

```command1 | command2```

Pipe commands takes the output of command1 and is used as input for command 2.

# 2.2.3 Script IO.

Propting users for input can be done, in bash, using the `read` command and can set multiple variables at once. csh uses   `set var=$<` and only sets one at a time.

``` bash
    read var1, var2, var 3
```
and will be seperated by whitespace characters.

File descriptors can be closed using `exec 0>&-`. <- We closed the FD $0

Descriptors can also be opened using `exec 1>&script_output.txt` This will create a file that was outputed from a created file descriptor.

> For creating sockets/connections. Simply use `/dev/tcp/host/port` and bash will connect to the host and the specified port (or will attempt). Connect only, no bind. BASH ONLY