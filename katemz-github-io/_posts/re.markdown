---
layout: post
title:  "Welcome to Jekyll!"
date:   2021-12-06 22:21:07 +0100
categories: jekyll update
published: false
---

objdump -D a.out | grep -A20 main.:


0000000000001149 <main>:
    1149:	f3 0f 1e fa          	endbr64 
    114d:	55                   	push   %rbp
    114e:	48 89 e5             	mov    %rsp,%rbp
    1151:	48 83 ec 10          	sub    $0x10,%rsp
    1155:	c7 45 fc 00 00 00 00 	movl   $0x0,-0x4(%rbp)
    115c:	eb 10                	jmp    116e <main+0x25>
    115e:	48 8d 3d 9f 0e 00 00 	lea    0xe9f(%rip),%rdi        # 2004 <_IO_stdin_used+0x4>
    1165:	e8 e6 fe ff ff       	callq  1050 <puts@plt>
    116a:	83 45 fc 01          	addl   $0x1,-0x4(%rbp)
    116e:	83 7d fc 09          	cmpl   $0x9,-0x4(%rbp)
    1172:	7e ea                	jle    115e <main+0x15>
    1174:	b8 00 00 00 00       	mov    $0x0,%eax
    1179:	c9                   	leaveq 
    117a:	c3                   	retq   
    117b:	0f 1f 44 00 00       	nopl   0x0(%rax,%rax,1)
    
    
    Dump of assembler code for function main:
=> 0x0000555555555149 <+0>:	endbr64 
   0x000055555555514d <+4>:	push   rbp
   0x000055555555514e <+5>:	mov    rbp,rsp
   0x0000555555555151 <+8>:	sub    rsp,0x10
   0x0000555555555155 <+12>:	mov    DWORD PTR [rbp-0x4],0x0
   
   
   This assembly instruction will move the value of 0 into memory located
at the address stored in the EBP register, minus 4.


   0x000055555555515c <+19>:	jmp    0x55555555516e <main+37>
   0x000055555555515e <+21>:	lea    rdi,[rip+0xe9f]        # 0x555555556004
   0x0000555555555165 <+28>:	call   0x555555555050 <puts@plt>
   0x000055555555516a <+33>:	add    DWORD PTR [rbp-0x4],0x1
   0x000055555555516e <+37>:	cmp    DWORD PTR [rbp-0x4],0x9
   0x0000555555555172 <+41>:	jle    0x55555555515e <main+21>
   0x0000555555555174 <+43>:	mov    eax,0x0
   0x0000555555555179 <+48>:	leave  
   0x000055555555517a <+49>:	ret    

    
    
MEMORY ADDRESS(hex) INSTRUCTIONS


AT&T ASM syntax - $, % prefixy


-M Intel do objdump: Intel syntax


gdb ./a.out
break main
run
info registers

RAX, RBX, RCX, RDX - general purpose registers in Intel64
Accumulator, Base, Counter, Data

RSI, RDI, RBP, RSP - also general purpose registers 
source index, destination index, base pointer, stack pointer

RIP - instruction pointer, points to current memory of operation

gdb
set disassembly intel

Intel Format:
operation <destination>, <source>
The destination and source values will either be a register, a memory
address, or a value.

ASM:
mov
sub
inc

cmp DWORD PTR[ebp-4], 0x9 // compare value located at address ptr[ebp-4] with 9
jle 8048393 <main+0x1f> // if less or equal, jump to 8048393
jmp 80483a6 <main_0x32> // otherwise jump 80483a6 (unconditionally)


gcc -g - add more debug info

gdb -q ./a.out
list
disassemble main
break main
run
info register rip

rip            0x555555555149      0x555555555149 <main>
// why is it not equal to main address?


(powtorzyc - skladnia x/s.. itp)
(gdb) x/3i $rip
=> 0x555555555149 <main>:	endbr64 
   0x55555555514d <main+4>:	push   rbp
   0x55555555514e <main+5>:	mov    rbp,rsp

nexti
bt - backtrace
print X
















