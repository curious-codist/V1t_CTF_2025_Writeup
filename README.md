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
