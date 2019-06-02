# Control Structures

## Conditionals

If statements are conditionals to check a specific condition. Below is an example of an if.
 - You can have only one _if_ and _else_ statement. However, you can insert as many _else if_ as needed to check multiple conditions. 

 ``` bash
    #!/bin/bash

    MYVAL=1
    if [ $MYVAL -eq 1 ]; then
        echo "value is 1"
    else
        echo "value is not one"
    fi

    MYVAL="a"
    if [$MYVAL == '1' ]; htne
        echo "value is 1"
    elif [ $MYVAL == 'a' ]; then
        echo "value is a"
    else 
        echo "value is neither 1 nor a"
    fi

    MYVAL=2
    if ! [ $MYVALUE -eq 1 ]; then 
        echo "Value is not one
    fi

```
-  The exclamation will reverse the conditional. In the above example the if is always created and concluded with _fi_. The exclamaion point reverses the statement. If MYVALUE == 2, it will be true, so `!TRUE` equates to `FALSE` 

``` bash
    if [ someCondition == true]; then
        echo "Perform some operation"
    fi
```

  -  Multiple if statements can be nested.

- Swtich statement is like an if, but different. whereas it will check conditions of some nature. 

``` bash
    swith [value]
    case value1:
        do Somehtin
        break;
    case value2:
        do other things
        break;
    default:
        do the thing as a default.
        break
```

Where `default` will be the action performed if no other conditions were met.

``` bash
    #!bin/bash
    case $1 in
        val1 )
            echo val1 - breaking
            ;;
        val2 )
            echo val2 - falling through
                ;&
        * )
            echo default
            ;;
    esac
```

File test conditionals. Below overlaps between `bash` and `csh`

|conditionals (files) |
| --- |
| -e True if file exists |
| -f True if file exists and is a regular file |
| -w True if file exists and is writable |
| -x True if file exits and is executable |
| -d True if file exists and is a directory |

> For a full list of conditionals, file and strings, check out [this cheat sheet](https://devhints.io/bash)

#
## Loops

Loops look nearly the same in almost all of the shells. Below are syntax for Bash, Csh, Ksh:

``` bash
    while [ condition ]
    do
        commands
    done
```

``` csh
    while (condition)
        commands
    end
```

``` ksh
    while [[ condition ]]
    do
        commands
    end
```

Like if/else statements, it is based off of conditions. Once the condition is established, it is checked. Once the action is performed, the condition is checked again. It continues until the condition is met. Below are syntax examples for the loops:

``` bash
    #!/bin/bash
    COUNTER=0
    while [$COUNTER -lt 10 ] 
    do
        echo Value is: $counter
        (( counter++ ))
    done
```
- The above code will print up intil the number 10.

For loops operate with values in a set, array, dictionary, or list.

Bash/ksh
``` bash
    for val in set
    do
        commands
    done
```

csh
``` csh
    foreach val (set)
        commands
    end
```
A set can be defined in a few different ways. Sets can be explecitly defined such as 1-5. In bash/ksh, a set can be defiined through bracket expansion. ` {1..5}` or `seq 1 5`

A set can also be created by file expansion. This makes it very easy to perform an operation on each file, within a directory.

Examples: 

``` bash
    #!/bin/bash

    for counter in {1..10}
    do
        echo Value is: $counter
    done
```

The below example will show each file, within the directory called `/temp/*` (similary, to view specific files, such as txt, `/temp/*.txt`)
``` bash
    for file in /temp/*
    do
        echo "File:" $file
    done
```

To stop a loop early, the `break` keyword can be used. If multiple loops are nested, an integer can be placed behind the command, and it will break that many loops. For example, if there is 3 nested loops, and the break happens within the third loop. `break 3` can be used to break all of them.

#

Some examples.

- For example, loops in java may look somethin like:

``` java
    for (int i = 0; i < 10; i++) {
        counter++;
    }
```

- Similarly, same can be done in bash.

``` bash
i=0
while [ i -lt 10 ]; then
do
    (( i++ ))
done
```

To perform sequences in bash:

``` bash
for num in 1 2 3 4 5
do
    command
done
```

**Note that `seq 1 100` can be used as well**