# Low Key
**Category**: Cryptography
**Points**: 150
**Author**: Kishor Bhat ([kbhat95](https://github.com/kbhat95))
> Hey!

> I just met you!

> And this is craazy!

> But here's a product of prime numbers!

> Try me maybe!

> 2449

> P.S. My mother told me that the 7th key fits in the lock. ;)

> Hint: Hint is implied in the question.

> <insert link to LowKey.tar.gz>

## Write-up
The 'product of prime numbers' and the fact that it's under cryptography indicate it is a RSA problem.

2449 is the public modulus (n), and 7 is the public exponent (e). Using these, either by applying Euclid's extended
algorithm yourself or using an online calculator for the same (this way's easier), you can find the private exponent (d).

'whatami.txt' in the tarball contains the encrypted message. With the knowledge of 'd', any RSA decrypter (online or otherwise)
will return the decrypted message: flag{shamir_would_be_proud}

> Flag: shamir_would_be_proud
