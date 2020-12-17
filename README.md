# some-tryhackme
i am broke
## Reversing ELF
Room for beginner Reverse Engineering CTF players, a.k.a, me
### crackme1
literally just fucking run it, flag{not_that_kind_of_elf}
### crackme2
fucking *strings* it
```
$strings crackme2
.
.
super_secret_password
.
$./crackme2 super_secret_password
Access granted.
flag{if_i_submit_this_flag_then_i_will_get_points}
```
### crackme3
```
$strings crackme3
.
.
ZjByX3kwdXJfNWVjMG5kX2xlNTVvbl91bmJhc2U2NF80bGxfN2gzXzdoMW5nNQ==
.
```
using **CyberChef**, base64
```
f0r_y0ur_5ec0nd_le55on_unbase64_4ll_7h3_7h1ng5
```
## crackme4
Nothing showed upfrom *strings*
```
gdb) disassemble main
Dump of assembler code for function main:
.
.
0x0000000000400754 <+62>:    callq  0x40067a <compare_pwd>
.
(gdb) disassemble compare_pwd
.
.
    0x00000000004006cf <+85>:    mov    rsi,rdx
   0x00000000004006d2 <+88>:    mov    rdi,rax
=> 0x00000000004006d5 <+91>:    call   0x400520 <strcmp@plt>
```
A litte strcmp between, RSI and RDI
```
(gdb) i r
rsi            0x7fffffffe74f      140737488349007
rdi            0x7fffffffe3d0      140737488348112
.
.
(gdb) x/s 0x7fffffffe3d0
0x7fffffffe3d0: "my_m0r3_secur3_pwd"
```
## crackme5
now a scanf, kill me

found a little strcmp(rdi, rsi)
```
    0x0000000000400829 <+182>:   mov    rsi,rdx
    0x000000000040082c <+185>:   mov    rdi,rax
=>  0x000000000040082f <+188>:   call   0x4006d6 <strcmp_>
.
.
(gdb) x/s 0x7fffffffe3e0
0x7fffffffe3e0: "OfdlDSA|3tXb32~X3tX@sX`4tXtz"
```
what kind of fucking string

if JNE (rax != 0), jump to 0x400849 <main+214> which displays
```
(gdb) x/s 0x400973
0x400973:       "Always dig deeper"
```
the shit we wanna display is
```
(gdb) x/s 0x400969
0x400969:       "Good game"
```
to get there, strcmp should return 0x0 so by inputting the weird string **OfdlDSA|3tXb32~X3tX@sX`4tXtz**
```
(gdb) i r
rax            0x0                 0
```
strcmp returns 0 and "Good game" is outputted
```
(gdb) c
Continuing.
Good game
```
## crackme6
```
password "%s" not OK
Usage : %s password
Good luck, read the source
```
another fucking **compare_pwd**, fucking get more creative names
```
0x000000000040074f <+62>:    call   0x4006d1 <compare_pwd>
```
the another
```
    0x00000000004006e1 <+16>:    mov    rdi,rax
    0x00000000004006e4 <+19>:    call   0x40057d <my_secure_test>
```
this fucking one is fucking huge, kill me
```
