# Shell Configuration Reconn: 2.x.x
 
 # 2.1.1

- Determine what shell is being used. Sometimes, you're unable to switch shells. can find the 
- A variable called 0 allows us to look up what shell we're using. 
```  bash
 less /etc/passwd
 echo $$
 echo $0
```

- Internal (builtin) runs as built in. External looks for other executable command that user created. 
- echo $PATH looks for an execuatble path. A practical example why we should care about path.

- Command History. More cyber reason is to see what other people have done. Name of the file is different for each shell. Can view by looking at $HISTFILE. All commands are sent here. To run commands agin run ``` !! ``` runs the last command. or using ``` !164 ``` will run from the history. 
### Basic commands
- cat /etc/passowrd
- ssh user@freebsd9 --> cat /etc/passwd

During login the program sets an environment variable,``` SHELL ```, which can be quired. the command would be somehtin as ```echo $SHELL```. That will give an output simular to ``` /bin/bash``` or ``` /bin/sh```

  - Another helpful command will be ``` echo $0; which $0 ```exils 

## Shell Configuration
------

Shells can be configured by startup scripts. These are script files that define aliases, source variables, and set variables.

- The systemwide ```/etc/csh.cshrc``` is first executed, followed by ```/etc/csh.login``` if it is a login shell. It will then execute the user specific ```~/.cshrc``` script and, if it is a login shell, the ```~/.login``` script.

***Note that Bryan gave me his ``` ~/.bashrc ``` scirpt at DXC***

# 2.1.3 Script execution

```#!/bin/bash``` This is a special directive that tells which interpreter is going to run the script. Bellow is a bash script called HelloWorld.sh

``` bash
 #!/bin/bash
 echo "Hello World
```
The user will need to be within the same directory as the execuatable. However occasionally an issue will arise where the current user won't have the proper permissions to execute.
- To check the permissions of the file, run the ls -l HelloWorld.sh command to check the permissions. The command and output should look something like: 

``` 
    MINGW64 ~/Documents/Misc$ ls -l HelloWorld.sh

    -rwxr-xr-x 1 willi 197609 32 May 10 10:33 HelloWorld.sh*
```
| User (owns) | Group | Others (on the system)|
| --- | --- | --- |
| -rwxr (read, write, execute) | -xr (execute, read) | -x (execute) |

  -  Numerical representation when assigning perfmissions: R (4) W (2) X (1). The digits are added together for a max of 7.
      - This means that 7 is rwx, 6 rw, and 4 r. 
      - Permissions are changed through `chmod`. So to retain the permisions above: `chmod 764` where the numbers are (user, group others)
      
# 2.1.5 Variables

To creat a variable can be created by `var_name=`. In csh will need to set. `set var_name=SomeValue`

More useful way is using command subsitutuion. Such as 
```
test_var2=`whoami`
```

To refrence variable names, can use $var_name. To view the variable from the command (outside a script.) Use echo such as `echo $var_name`.

 ##### *Using a plus sign (+) will put an actual plus sign. To prepend, or concatenatel you can use the word "plus". `var_name=Hello plus second part. --> "Hello second part*

 If numbers are being used, on shell and bash will need to use `expr 10 + 20`
 ``` bash
    a=10
    b=20
    c=`expr $a+$b`
    echo $c
 ```
 The out put will be 30. ** Can also use `let`.

 Within csh, we will need to use `@`

 ``` csh
    set a=10
    set b=20
    @ c = $a + $b
    echo $c
 ```

 *Varible scopes* are environment and local. Environment effects all shells on the system. Local effects the single shell.

 For bash, create a variable then export it.

 ``` bash
    TEST='TEST VARIABLE'
    echo $TEST
    bash
    echo $TEST
    export TEST.
 ```
 - This allows you to refrence the variable from a new bash instance.
 
 Similarly, in csh we use `setenv` command.
 ``` csh
    setenv TEST 'TEST VARIABLE2'
    csh
    echo$TEST
    bash
    echo $TEST
 ```

 To pass variables, we can pass it into a new instance in bash. Another way is called sourcing. It's used alot in startup scripts (./bashrc).
 Ex:
 ``` bash
    !#bin/bash
    MYVAR='variable dos'

    echo Sourcing 1...
    source script1
    echo Var1 is $MY_VAR
    echo VAR2 is $MY_VAR
 ```
Then you can create an executable `chmod +x SomeScript.sh`

Regarding passing `$0` is script name. `$1, $2...$n-1` is the number of arguements. Whereas `$@` is all arguments passed as an array.

``` bash
   #!/bin/bash

   ls -l /etc/profile.d # 1
   ls -l /etc/rpm       # 2

   netstat --tcp       # 3
   SYSTEM="$(uname -a)" # 4
   echo $SYSTEM         # 5

   echo $@              # 6
```

 1. Lists the profile of scripts to run.
 2. Lists the rmp database
 3. Lists all tcp connections (active and inactive). View through netstat
 4. Looks up user and assigns to SYSTEM variable name
 5. Prinsts the SYSTem variable 
 6. Lists all arguments passed to the program. 
