# String Manipulation

## Regular Expressions

As most everyone should know, regular experessions are a sequence of symbols and characters expressing a string or pattern to be searched for within a longer piece of text. For more of a relaxed term, a set of rules to which testing a if a string would match, given it has the same properties and meets the criteria.

Metacharacters Are special characters that are used by regex. These characters is what give Regex's their power. Below are some examples:

| Quantifiers | Meaning |
| --- | --- | --- |
| + | One or more |
| {x} | exactly 'x' times |
| {2, 4} | exactly 2 to 4 times |
| {3,} | appears 3 or more times |
| * | Zero or more times |
| ? | Once or none | 

[**This is a full RegEx cheatsheet**](https://www.rexegg.com/regex-quickstart.html)

RegEx can be used to solve problems. For example finding a credit card. `\b(\d[ -]*?) {13,16}\b` is an example. Lets break it down.

- `\b` is a word boundry. Since it is at the end of each side, saying to find words between the two. 

- Within the `()` creates a capture group and treats the characters within it, as a group and keep them together.

- `\d` Find any number, the [ -] says find a space or a dash. `*` find zero or more of these. with `?` plac3ed behind, means lazily find the precedings. In total, what is between the parenthesis, _Find the number, followed by space or dash, as few times as possible_

- `{13-16}` means to find the proceding at a min of 13, and up to 16 times.

Regular expresions are initially very difficulet to grasp and takes time to become remotely proficient. It is however a very great skill to have and makes thinks much easier in almost every programming language.

#

## Text Manipulation

The RegEx patterns can be combinied to achieve different objectives. For example, displaying certian parts of a file. A couple of tools are `cut` and `awk`.

- The cut program lets you easily set a delimiter and display what part of the file to display. Ex:

``` bash
    root$plc:~# cut -d: -f1 /etc/passwd
```

Displays filed 1

- The awk command has its own scripting language. One feature is displaying only specific parts of text. Ex:

``` bash
    root$plc:~# awk 'BEGIN {FS=":"} {print $1}' /etc/passwd
```

- Within the quotes, we tell it to begin, using the field seperator delimiter to sperate using a colon. In the other, we tell awk what field to print. It used RegEx to seperates field. It also handle whitespace and trailing character better than `cut`.

To Modify text, can use `tr, awk,` and `sed`. Similar to how find and replace works in programs, such as microsoft word. 

- `tr` generally stands for translate. An example below swaps characters from lower case, to upper case.

``` bash
    root$plc:~# awk '{print $1}' /etc/fstab | tr [:lower:] [:upper:]
```
**fstab stands for 'file system table' and is used to review file systems.**

- Using sed, stream editor, it allows you to swap characters easier. While tr lets you translate _character for character_, sed lets you translate word for word. for example, changing the word "was" to "is". Sed also supports Regular expressions.

 Example:

``` bash
    root$plc:~# head spam_list.txt | sed -r 's/[a-z]+@[a-z]+.com/REDACTED/g'
```
-  The above example, using regex, will look for email addresses and replace them with the word `REDACTED`.

    -  The awk command can also support this functionality. Because awk has its own scripting language, it can do much more.

The `sort` and `uniq` command allows you to sort text. 

- Sort allows you to sort alphabetically, reverse alphabetically, numerically, by month, or random order

Example:

``` bash
    root$plc:~# head spam_list.txt | sort
```

- the uniq command allows you to remove duplicate lines of text.

``` bash
    root$plc:~# head spam_list.txt | uniq 
```

> The 'head' command will only show the first 10 or so lines with the file. Similarly, the 'tail' command will show the last 10 or so lines of the file. Using the `|`, as you may remember, takes stdout from one command as stdin into the second. (cmd1 | cmd2)

Lastly, another useful tool is `grep -v`. Grep is a utility for searching patterns within a file. Viewing the manual of grep, you can see that the option `-v` looks for the inverse of the pattern. It's useful for filtering out results. For example, if you want to see what users can log into a system, use the following command to view who can't login.

```
    root$plc:~# grep nologin /etc/passwd
```
- Using the inverse, you can filter who can login

```
    root$plc:~# grep -v nologin /etc/passwd
```