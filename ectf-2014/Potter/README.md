# Potter
**Category**: Exploit
**Points**: 250
**Author**: [Ajith Pandel](https://github.com/ajithps)
> You have been able to get your hands on the flag storing program of ECTF flag system. But you don't have the required libECTFsecret.so file.<br/>
Can you still bypass the system?

> [Potter.zip](./Potter.zip)

> nc 212.71.235.214 3050 

## Write-up
We are given a archive named Potter.zip . It contains a single executable called potter

Connecting to the service, we see that it some kind of menu based program. It has options for Admin Access, View flag, Set new flag and exit.

Trying out the options tells us that to get the flag stored we need to somehow get admin access. Otherwise a blank flag{} gets printed, and it announces that we don't have admin access.


Reversing the _main_ function tells us that it is a switch case program. The admin access is called if we enter 1 as the input to the menu. Which is what we see when we connect to the service using nc. Looking at the arguments passed to the admin function, we see that the same thing is passed to _set\_flag_ , _view\_flag_ , _admin\_access_ except that the function _view\_flag_ is called with only one argument which is common to all other functions.

The _main_ function inititally calls _setup_ function with two arguments which are pointer to some local variables var1 and var2. Looking at the arguments passed to other functions we note var1 is passed to all functions as the first argument, and var2 is passed as the second argument to both _admin\_access_ and _set\_flag_ function.


Reversing the _setup_ function tells us it does something like this

<br/>
> setup (void \*\*var1, void \*\*var2)<br/>
> {<br/>
>	var1 = malloc (52);<br/>
>	var2 = malloc (25);<br/>
>	check\_and\_load\_flag(\*var1);<br/>
>	load\_pass(*var2);<br/>
> }<br/>
<br/>

We get to know that var1 and var2 in the main function are actually pointer variables.

Reversing the _admin\_access_ function and through the hex dump of the binary we come to know that the function does something like this.

> if (some\_var[50] == 'A')<br/>
>   puts("Admin access granted");<br/>
> else<br/>
> {<br/>
    // Ask for password and authenticate<br/>
> }<br/>
><br/>
> if (some\_var[50] == 'A')<br/>
>   check\_and\_load\_flag(some_var)<br/>
> <br/>
<br/>

The some_var is saved at 0x8(%ebp) during the admin\_access function scope. It is a pointer passed to this function. Since it is saved at 0x8(%ebp) we can say that it is the first argument to this function.

i.e The program will assume that the user has admin access if var1[50] == 'A'

Reversing the _convert_ function tells that it is simple conversion of any numerical string ( eg:"123" ) to decimal equivalent (eg:123);

Reversing the _set\_flag_ gives us some code like this

> set\_flag(void \*var1, void \*var2)<br/>
> {<br/>
>   int var3;<br/>
>   while (1)<br/>
>   {<br/>
>       scanf two strings        <br/>
>       convert the first string to number(var3)<br/>
>        <br/>
>       if (var3 <= 50 && var3 >= 0)<br/>
>       {<br/>
>           var1[var3] = first_string[0];<br/>
>           puts("Set");<br/>
>       }<br/>
> <br/>
>       if (var3 == 100)<br/>
            call admin access<br/>
>    }// End of while<br/>
> }<br/>


This means that _set\_flag_ function is allowing us to set the admin\_check variable (var1[50]) also!! 

So we directly connect to the service through nc and use the 3) Set new flag option.
There we give the index as 50 and ch as 'A'

Exit the set flag option by giving input 100 0

Input 0 to refuse saving the flag.

In the main menu go to admin access to make the program call the _check\_and\_load\_flag_ function since we "have" admin access.

Again in the main menu give the option to view the flag to get the flag!

> Flag: flag{The\_Flying\_Phoenix}

## External Write ups

