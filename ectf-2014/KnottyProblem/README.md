# A Knotty Problem
**Points**: 250
**Category**: Exploit
**Authors**: [Ajith Pandel](https://github.com/ajithps), [Mithesh Mutha](https://github.com/mitesh-mutha)
> When you reach the end of your rope, tie a knot in it and hang on - Franklin D. Roosevelt

> [Knotty.tar.gz](./Knotty.tar.gz)

> nc 212.71.235.214 5000 

## Write-up
We are given a single archive named _Knotty.tar.gz_ which contains a single executable named _knotty_.
Looking at assembly code of the executable we see that there are three functions

* _main_ function
* _dummy_ function
* _get\_string_ function

The _main_ function just calls the _dummy_ function and exits.

The _get\_string_ function takes input from the user through _gets_ and returns.

The _dummy_ function is never called but there is system call inside it. It passes /bin/date string as an argument.

We basically need to somehow make the program drop us into the shell so that we can read the flag file.

For that we need a system("/bin/sh") call to be executed.

Running `strings` on the binary tells us that there is string `/bin/bash -c "cat flag.txt"` somewhere in the binary. This is exactly what we need!!

So to find the address of that string we run the `xxd` command. Through that we see that the above string is there at a offset of 0x5c0 from the starting of the binary. So the address of the string is 0x080485c0 . We can confirm this by running gdb and printing the string that is present in the address.

Since we now have the address of the argument as well as the address of the system call, we just need to derive our input. _dummy_ function tells us that _gets_ will write to EBP - 0x88 address. The address of system call is 0x080484da


Thus our input will be

"A"*136 + "JUNK" + `system_call_address` + `address_of_argument`

This reduces to "A"*140 + "\xda\x84\x04\x08"+"\xc0\x85\x04\x08"

Our final exploit is
<code>
> $ python -c 'print "A"*140+"\xda\x84\x04\x08"+"\xc0\x85\x04\x08"' | nc 212.71.235.214 5000<br/>
> flag{assembly\_is\_awesome!!}
</code>


> Flag: flag{assembly\_is\_awesome!!}

## External Write ups

