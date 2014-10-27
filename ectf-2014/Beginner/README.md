# The Beginner
**Points**: 100
**Category**: Exploit 
**Authors**: [Ajith Pandel](https://github.com/ajithps), [Mithesh Mutha](https://github.com/mitesh-mutha)
> The expert in anything was once a beginner

> [beginner.tar.gz](./beginner.tar.gz)

> nc 212.71.235.214 2000 

## Write-up
The file beginner.tar.gz contains one file named beginner which is a plain text file.<br/>
We observe that the file is a [objdump](http://man7.org/linux/man-pages/man1/objdump.1.html) \[1\] file.

Lines 191 - 204 represents the _main_ function.<br/>
Lines 181 - 189 represents a function titled _easy\_function_<br/>
Lines 159 - 179 represents a function titled _print\_flag_<br/>


A little amount of reversing tells us that the _main_ function just calls _easy\_function_. And _easy\_function_ takes input from the stdin using _gets_ standard C library function. The function _print\_flag_ is never called.

We can exploit the _gets_ library function to overwrite the return address of _easy\_function_. Such that it calls the function _print\_flag_ . [\[2\]](http://www.tenouk.com/Bufferoverflowc/Bufferoverflow4.html)

###Few points to be noted. <br/>
* The address of _print\_flag_ is 0x0804849d (Line 159 of _beginner_ file)<br/>
* The starting address of the variable to which _gets_ is saving the input is 0x1c (28) less than the address to which EBP is pointing. (Line 185 of _beginner_ file)

The stack can be visualised like this [\[3\]](http://en.wikipedia.org/wiki/X86_calling_conventions#cdecl)

ArguementsToFunctionIfAny | ReturnAddress | PrevEBP | SpaceAllocatedByFunction |-->StackGrowth

* EBP points to address where the previous value of EBP is stored.(Line 182 and 183)

_gets_ writes to address determined by (EBP - 28). Since we have to overwrite the return address of the easy\_function, we should give an input which is something like this 28chars + 4chars + _AddressOf(easy\_function)_

The 29th-32nd input characters will overwrite the PrevEBP and anything after that is written over the return address of the _easy\_function_

* The address should be supplied in little endian and are 4 bytes long. (Line 2 of _beginner_ file)

Thus the address of _easy\_function_ (0x0804849d) should be supplied in reverse. i.e \x9d\x84\x04\x08

Hence the input should be "A"*(28 + 4) + "\x9d\x84\x04\x08"

This command would output the same.

<code>
> $ python -c 'print "A"*32+"\x9d\x84\x04\x08" '
</code>

Thus the final exploit:<br/>
<code>
> $ python -c 'print "A"*32+"\x9d\x84\x04\x08" ' | nc 212.71.235.214 2000<br/>
> flag{this\_is\_just\_the\_beginning}
</code>

###Links:<br/>
1. [http://man7.org/linux/man-pages/man1/objdump.1.html](http://man7.org/linux/man-pages/man1/objdump.1.html)
2. [http://www.tenouk.com/Bufferoverflowc/Bufferoverflow4.html](http://www.tenouk.com/Bufferoverflowc/Bufferoverflow4.html)
3. [http://en.wikipedia.org/wiki/X86\_calling\_conventions#cdecl](http://en.wikipedia.org/wiki/X86_calling_conventions#cdecl)


> Flag: flag{this\_is\_just\_the\_beginning}

## External Write ups

