# Batman
**Points:** 300
**Category:** Crypto
**Author:** Kavya Vishwanath
> I'm derived with several alternating levels of substitutions and permutations

> Hint : This superce**des**

## Write up

Steps :

1. Identify the encryption algorithm AES
2. Observe for same username with different password the token is same , which implies the token is dependent only on the username
3. Identify the block size , observe the first 16 bytes are constant for any username , 16 is the block size .
4. Try to observe the pattern for varying input pattern length like “aaaaaaaa”, “aaaaaaaaaaa.....” etc
5. Identify a block which fits “admin” username

For example :

Username : aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa

Hexdump :

0000: 69 75 71 77 65 51 39 31 32 6d 50 71 55 46 71 6b |iuqweQ912mPqUFqk|
0010: fd aa 4e b5 da 6c 06 ad 94 2b b9 a8 22 28 69 c6 |ýaNμÚl..+1 ̈"(iÆ|
0020: fd aa 4e b5 da 6c 06 ad 94 2b b9 a8 22 28 69 c6 |ýaNμÚl..+1 ̈"(iÆ|
0030: af b1 b2 6d 6b 00 e5 44 22 b3 0a ab 14 c9 54 7a | ̄±2mk.åD"3.«.ÉTz|

Username : aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa (one 'a' more than the previous input creates one more extra block as seen the below hexdump , this can be leveraged to find the encdoing for admin )

Hexdump :

0000: 69 75 71 77 65 51 39 31 32 6d 50 71 55 46 71 6b |iuqweQ912mPqUFqk|
0010: fd aa 4e b5 da 6c 06 ad 94 2b b9 a8 22 28 69 c6 |ýaNμÚl..+1 ̈"(iÆ|
0020: fd aa 4e b5 da 6c 06 ad 94 2b b9 a8 22 28 69 c6 |ýaNμÚl..+1 ̈"(iÆ|
0030: fd aa 4e b5 da 6c 06 ad 94 2b b9 a8 22 28 69 c6 |ýaNμÚl..+1 ̈"(iÆ|
0040: 1b f4 7e e4 a0 43 66 e8 6a d1 f9 55 c7 81 d0 f9 |.ô~ä CfèjÑùUÇ.Ðù|

Username : aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaadmin

Hexdump :

0000: 69 75 71 77 65 51 39 31 32 6d 50 71 55 46 71 6b |iuqweQ912mPqUFqk|
0010: fd aa 4e b5 da 6c 06 ad 94 2b b9 a8 22 28 69 c6 |ýaNμÚl..+1 ̈"(iÆ|
0020: fd aa 4e b5 da 6c 06 ad 94 2b b9 a8 22 28 69 c6 |ýaNμÚl..+1 ̈"(iÆ|
0030: fd aa 4e b5 da 6c 06 ad 94 2b b9 a8 22 28 69 c6 |ýaNμÚl..+1 ̈"(iÆ|
0040: c8 18 65 84 f1 c6 4b 58 18 47 d5 71 39 ee 5b b8 |È.e.ñÆKX.GÕq9î[ ̧|

From the above examples it is clear that the token for admin can be identified by combining

69 75 71 77 65 51 39 31 32 6d 50 71 55 46 71 6b and c8 18 65 84 f1 c6 4b 58 18 47 d5 71 39 ee 5b b8 , generate the base64 for this , which is the required token .

> Token : aXVxd2VROTEybVBxVUZxa8gYZYTxxktYGEfVcTnuW7g=
> flag{A3S_s0_s3cure}

## External Write ups
