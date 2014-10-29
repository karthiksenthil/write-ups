# Batman
**Points:** 100
**Category:** Crypto
**Author:** Kavya Vishwanath
> Batman is on the social networking site , he doesn't reveal his name. Instead, for every user who requests his name he replies in a unique way (he gives a name specific to the person asking for his name) so do you know Batman's name ?

## Write up
The encryption is based on Vigenere Ciphers . It is a simple form of polyalphabetic substitution . Try to find the pattern of shift in the alphabets for different user­name inputs.

For example :

>input : jkuiopqw
>output : bylzmxtkwdbijtqjjwy
>input : uiollja
>output : mwfcjrdivhsleeuvoxp

From observations its clear that the string is of length 20.

#### Decryption :

Consider first input , convert this string to a string of length 20 by concatenating ,jkuiopqwjkuiopqwjkui and the ouput corresponding to this is bylzmxtkwdbijtqjjwy , the index of j when the alphabets [a­z] are numbered [0­26] is 9 , so when decrypting a the string one has to traverse back 9 steps from b which gives s , which is the first character of the same string ,similarly ind(k)=10 , decrypt(y) = o , ind(u)=21 , decrypt(l)=r , following the same procedure for other characters in the input the key to be identified will be “sorryidonthaveaname”

> flag{w4yne_cRacks_ciph3r}

## External Write ups
