# shell function: and
Shell operators are useful to invoke multiple commands.
```shell
 % command1 && command2 && command3
```
But in our shell life, it's not always easy to do that.

After I started one command, I sometimes want to invoke another command when the previous command succeeds.
The previous command has been running for hours and I don't want to interrupt it.
I have something to do and leave my laptop, for lunch or a coffee break, etc.

This note suggests the following shell configuration.
```shell
and() { [ $? -eq 0 ] && "$@"; }
```
This function allows us to invoke commands only when the previous command succeeded or not.
It's just an alias to testing the last exit status and the logical operator `&&`, but I'm lazy that I don't want to type that much and nobody cares how abbreviated my own alias configurations are.

Using the `and` function, I let the shell to invoke another command while the previous command is running.
```
 % command1
running...
and command2
and command3
```
The shell is listening to what I typed and will invoke `command2` only when `command1` exits normally.
For example, consider upgrading all the packages.
```
 % brew update
 % and brew upgrade
 % and brew cleanup
```
Of course, be sure that the commands do not accept the standard input.
If they may change the behavior based on the standard input, I have no responsibility what happens.

There's another configuration which is actually the logical counterpart.
```shell
or() { [ $? -eq 0 ] || "$@"; }
```
Using this function, `or command` invokes the command only when the previous command exit abnormally.

Lastly, don't introduce the weird synonyms in shell scripts.
I'm suggesting using these functions in shell operations just for ease.
