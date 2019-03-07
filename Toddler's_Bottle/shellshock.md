# Shellshock
Shellshock.c :
```
shellshock@ubuntu:~$ cat shellshock.c 
#include <stdio.h>
int main(){
	setresuid(getegid(), getegid(), getegid());
	setresgid(getegid(), getegid(), getegid());
	system("/home/shellshock/bash -c 'echo shock_me'");
	return 0;
}
```
As we see the classic shellshock vulnerability. 
### Explanation:
This original form of the vulnerability involves a specially crafted environment variable containing an exported 
function definition, followed by arbitrary commands. Bash incorrectly executes the trailing commands when it imports
the function.  (Source Wiki)
```
env x='() { :;}; echo vulnerable' bash -c "echo this is a test"
```
Therefore, `() { :;}` this is used as a function definition and everything after a `;`will be executed.
![alt text](https://i.imgur.com/KWcrYwG.png)
<br>and we get the flag.
