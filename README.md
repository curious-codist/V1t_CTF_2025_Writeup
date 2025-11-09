# V1t_CTF_2025_Writeup
Writeup for some of the problems I solved in the V1t CTF 2025
Also, the challenges can be found at [Here](https://ctf.v1t.site/challenges)

# Challenge 1 - Python 0bf(Rev)
In this challenge, we are given an .py file. Opening which, we are met by a zlib decompressing a reversed base64 string.
```python
_ = lambda __ : __import__('zlib').decompress(__import__('base64').b64decode(__[::-1]));
exec((_)(b'==Something Something'))
```

Reversing the string and decompressing it using a python script gives us decoded code which also contains an 
```exec((_)(b'==Something'))``` thing

Recursively doing this over and over, we are left with this
```python
flag = "v1t{d4ng_u_kn0w_pyth0n_d3bugg}"

inp = input("Input the flag: ")

if (inp != flag):
    print("wrong")
else:
    print("correct")
```

## Flag - v1t{d4ng_u_kn0w_pyth0n_d3bugg}


# Challenge 2 - Bad Reverser(Rev)
In this challenge, we are given a 64-bit executable. We gotta reverse engineer it to get the flag(obviously). Opening it in hex-rays, the major thing we see first is 
```c
_BYTE byte_2040[41] =
{-116,-47,-113,-114,-115,-116,-57,-113,-114,-116,-116,-45,-113,-114,-113,-116,-11,-113,-114,-114,-116,-56,-113,-114,
-119,-116,-98,-113,-114,-120,-116,-18,-113,-114,-117,-116,-41,-113,-114,-118,114}; // weak
```
s is a string with length 11. Now,
```c
v5 = 0;
v6 = (s[0] + 7) ^ 0x5A;
do
{
v21[v5] = v6 ^ byte_2040[v5] ^ 0xAA;
++v5;
}
while ( v5 != 41 );
while ( 1 ){
    v10 = v21[v8];
    v11 = v9 + 1;
    if ( v10 != 1 )
      break;
    v12 = v11;
    if ( v9 == 40 || v7 > 15 )goto LABEL_16;
    v13 = v7;
    v9 += 2;
    ++v7;
    v8 = v9;
    if ( v9 > 40 )goto LABEL_16;
}
```
We know s[0]='v'.This basically gives us v6 = 0x27. Now, we got that v21 as well. The while loop is basically kinda an interpreter, which does
- 1 imm → push imm
- 2 → xor top-of-stack with v6
- 3 idx → pop top and compare with s[idx + 3]
- 0xEE → skip
- 0xFF → success

From these comparision instructions, I extracted s[3] to s[10] which is {my_b4D}

## Flag - v1t{my_b4D}

