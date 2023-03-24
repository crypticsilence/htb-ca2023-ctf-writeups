Checked this out in ghidra. Quick and easy chall.

First figured out the routines:
`
void reverse(char *str,char *str2,ulong length) {
  int i;
  for (i = 0; i < length; i = i + 1) {
    str[i] = str2[(length - i) -1];
  }
  return;
}

void xor(char *str1,char *str2,ulong len,byte xorbyte) {
  int i;
  for (i = 0; (ulong)(long)i < len; i = i + 1) {
    str1[i] = str2[i] ^ xorbyte;
  }
  return;
}

Main just calls Exam:

void exam(void)
{
  int input;
  undefined8 local_38;
  undefined8 local_30;
  undefined local_28;
  undefined8 empty;
  undefined4 local_14;
  char *hint1;
  hint1 = (char *)readline(
                          "Okay, first, a warmup - what\'s the first password? This one\'s not even  hidden: "
                          );
  input = strcmp(hint1,"PasswordNumeroUno");
  if (input != 0) {
    puts("Not even close!");
                    /* WARNING: Subroutine does not return */
    exit(-1);
  }

PasswordNumeroUno

First password is easy obvs. Second is a little diff?

But just reversing the var 't' gets us what we need I guess.

  free(hint1);
  empty = 0;
  local_14 = 0;
  reverse((char *)&empty,t,11);
  hint1 = (char *)readline("Getting harder - what\'s the second password? ");
  input = strcmp(hint1,(char *)&empty);
  if (input != 0) {
    puts("You\'ve got it all backwards...");
                    /* WARNING: Subroutine does not return */
    exit(-1);
  }
  free(hint1);`

Looked in ghidra, t is:

`t = b'\x30\x77\x54\x64\x72\x30\x77\x73\x73\x34\x50\x00'``

0wTdr0wss4P.

.P4ssw0rdTw0

ezpz..

`  t3 = 0;
  local_30 = 0;
  local_28 = 0;
  xor((char *)&t3,t2,0x11,0x13);
  hint1 = (char *)readline("Your final test - give me the third, and most protected, password: ");
  input = strcmp(hint1,(char *)&t3);
  if (input != 0) {
    puts("Failed at the final hurdle!");
                    /* WARNING: Subroutine does not return */
    exit(-1);
  }
  free(hint1);
  return;
}`

The third is super easy xor ^ 0x13 ..
`
`t3=''`
t2=b'\x47\x7b\x7a\x61\x77\x52\x7d\x77\x55\x7a\x7d\x72\x7f\x32\x32\x32\x13'`
477b7a6177527d77557a7d727f32323213
G{zawR}wUz}r.222.
^ 0x13 =
ThirdAndFinal!!!.

PasswordNumeroUno
.P4ssw0rdTw0
ThirdAndFinal!!!.

└──╼ $./license

So, you want to be a relic hunter?

First, you're going to need your license, and for that you need to pass the exam.

It's short, but it's not for the faint of heart. Are you up to the challenge?! (y/n)

y

Okay, first, a warmup - what's the first password? This one's not even hidden: PasswordNumeroUno

Getting harder - what's the second password? P4ssw0rdTw0

Your final test - give me the third, and most protected, password: ThirdAndFinal!!!

Well done hunter - consider yourself certified!

Hah, when I went to get the flag, there were more questions . One caught me off guard but I got it pretty quick w/ gdb and ldd:

└──╼ $nc 144.126.196.198 30789

What is the file format of the executable?

> ELF

[+] Correct!

What is the CPU architecture of the executable?

> 64-bit

[+] Correct!

What library is used to read lines for user answers? (`ldd` may help)

> libc.so.6

[-] Wrong Answer.

What library is used to read lines for user answers? (`ldd` may help)

> linux-vdso.so.1

[-] Wrong Answer.

What library is used to read lines for user answers? (`ldd` may help)

> libreadline.so.8

[+] Correct!

What is the address of the `main` function?

> 0x401172

[+] Correct!

How many calls to `puts` are there in `main`? (using a decompiler may help)

> 5

[+] Correct!

What is the first password?

> PasswordNumeroUno

[+] Correct!

What is the reversed form of the second password?

> 0wTdr0wss4P

[+] Correct!

What is the real second password?

> P4ssw0rdTw0

[+] Correct!

What is the XOR key used to encode the third password?

> 0x13

[+] Correct!

What is the third password?

> ThirdAndFinal!!!

[+] Correct!

[+] Here is the flag: `HTB{l1c3ns3_4cquir3d-hunt1ng_t1m3!}`