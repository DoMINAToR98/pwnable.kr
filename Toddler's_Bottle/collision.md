# Collision
After downloading the file via 
```
scp -P2222 col@pwnable.kr:col .
scp -P2222 fd@pwnable.kr:col.c .
```
col.c :
```
root@kali:~/Desktop/pwnable# cat col.c 
#include <stdio.h>
#include <string.h>
unsigned long hashcode = 0x21DD09EC;
unsigned long check_password(const char* p){
	int* ip = (int*)p;
	int i;
	int res=0;
	for(i=0; i<5; i++){
		res += ip[i];
	}
	return res;
}

int main(int argc, char* argv[]){
	if(argc<2){
		printf("usage : %s [passcode]\n", argv[0]);
		return 0;
	}
	if(strlen(argv[1]) != 20){
		printf("passcode length should be 20 bytes\n");
		return 0;
	}

	if(hashcode == check_password( argv[1] )){
		system("/bin/cat flag");
		return 0;
	}
	else
		printf("wrong passcode.\n");
	return 0;
}
```
As we see the input is being compared with `0x21DD09EC`, we need to pass this value but the problem is with length. <br>
Therefore we try `0x21DD09EC + \x00*16 ` (value (4 bytes) + 16 null bytes = 20)
```
root@kali:~/Desktop/pwnable# ./col `python -c "print '\xec\x09\xdd\x21'+'\x00'*16"`
bash: warning: command substitution: ignored null byte in input
passcode length should be 20 bytes
``` 
As we get a null byte error we try with \x01 but for that we need to do some calculation:
![alt text](https://i.imgur.com/7QVrCpx.png) <br>
Therefore  value `0x1dd905e8 + \x01*16 = 0x21DD09EC`  <br>
![alt text](https://i.imgur.com/ylMC7eM.png)

