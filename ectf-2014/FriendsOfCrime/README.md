# Friends Of Crime
**Points**: 200
**Category**: Reverse 
**Author**: [Ajith Pandel](https://github.com/ajithps)
> Get the flag, my friend.

> [FriendsOfCrime.tar.gz](./FriendsOfCrime.tar.gz)

## Write-up
**NOTE**: One can solve this challenge in multiple ways(I know 3 methods). This is one of the simpler ones.

We are given a archive called FriendsOfCrime.tar.gz . It contains a single stripped executable named _friends\_of\_crime_ . 

[This](http://reverseengineering.stackexchange.com/questions/1935/how-to-handle-stripped-binaries-with-gdb-no-source-no-symbols-and-gdb-only-sho) will help.

* The starting address of the _main_ function is 0x8048462

* Looking at the fag end of the _main_ function (Instruction at 0x80487ca) we see that the program calls a function without any arguments and then the return value of that function is passed to a function which prints it.

* In gdb add a break point at starting address of the main function. And directly force the program to jump to the above mentioned instruction(0x80487ca).

> (gdb) break \*0x8048462<br/>
> Breakpoint 1 at 0x8048462<br/>
> (gdb) r<br/>
> Starting program: <br/>
> Breakpoint 1, 0x08048462 in ?? ()<br/>
> (gdb) jump \*0x80487ca<br/>
> Continuing at 0x80487ca.<br/>
> flag{Care\_for\_one\_more?}<br/>

###Alternate methods
* We can reverse the function that we forced the program to call in the first method and get the flag from that.
* We can reverse the main function to see that the program takes a command line argument. It checks whether the command line argument can be represented as a decimal number(_N_). Then it goes on to check whether atleast one of (5_N_^2 + 4) and (5_N_^2 - 4) is a perfect square and whether the number _N_ is within a range. This is satisfied by a fibonacci number within the bounds checked by the program. Calculate any one of those fibonacci numbers(there are 8) and pass it to the program to get the flag.

> Flag: flag{Care\_for\_one\_more?}

## External Write ups