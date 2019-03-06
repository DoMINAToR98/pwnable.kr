#FD
After downloading the file via 
```
scp -P2222 fd@pwnable.kr:fd .
scp -P2222 fd@pwnable.kr:fd.c .
```
we get:
fd.c file :
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char buf[32];
int main(int argc, char* argv[], char* envp[]){
	if(argc<2){
		printf("pass argv[1] a number\n");
		return 0;
	}
	int fd = atoi( argv[1] ) - 0x1234;
	int len = 0;
	len = read(fd, buf, 32);
	if(!strcmp("LETMEWIN\n", buf)){
		printf("good job :)\n");
		system("/bin/cat flag");
		exit(0);
	}
	printf("learn about Linux file IO\n");
	return 0;

}
```
It requires us to enter a number.
```
root@kali:~/Desktop/pwnable# ./fd
pass argv[1] a number
```
As the name suggests we need to pass in fd which is a file descriptor. <br>
File descriptor had 3 values 
```
0 - standard input 
1 - standard output
2 - standard error
```
As it is subtracting the value from 0x1234 (4660 in decimal) and we need the value to be 0 so that we can input our string. <br>
![alt text](https://i.imgur.com/GsqzMvw.png?1)

