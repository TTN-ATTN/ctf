# WannaGame Freshman 
## FREE FLAG challenge
### Level: 
easy
### Description: 
This is a warmup challenge so I give everyone a free flag! Easy peasy lemon squeezy!

[free_flag.zip](https://github.com/TTN-ATTN/ctf/blob/main/WannaGame/freeflag/free_flag.zip)

Download and unzip the file, you get a file named free_flag.pcapng. Strings the pcapng file and you will find some interesting things:
```
Hi guys
echo "Welcome to W1"
Welcome to W1
echo "Here is your flag"
Here is your flag
echo "not this FL4G: http://bit.ly/3tjnLXy"
not this FL4G: http://bit.ly/3tjnLXy
echo "Here is the REALFLAG"
Here is the REALFLAG
echo "you need to decrypt it ^_^"
you need to decrypt it ^_^

```

I clicked the link and got rickrolled, haha.
Based on the message, it seems that our flag is encrypted and we need to find how to decrypt it.

Do more scrolling and you will find:

```
> Please give me your secret: 
i love Miku
> Here is your encrypted secret: t43SgKjInqK3xss=
echo $LMAO
echo $LMAO
HIHIHIHIHIHIHIHIHI
Qm2P
Qm2P
echo $FLAG | python3 encrypt
 ___       __   ________  ________   ________   ________  ___       __     _____  ________      
|\  \     |\  \|\   __  \|\   ___  \|\   ___  \|\   __  \|\  \     |\  \  / __  \|\   ___  \    
\ \  \    \ \  \ \  \|\  \ \  \\ \  \ \  \\ \  \ \  \|\  \ \  \    \ \  \|\/_|\  \ \  \\ \  \   
 \ \  \  __\ \  \ \   __  \ \  \\ \  \ \  \\ \  \ \   __  \ \  \  __\ \  \|/ \ \  \ \  \\ \  \  
  \ \  \|\__\_\  \ \  \ \  \ \  \\ \  \ \  \\ \  \ \  \ \  \ \  \|\__\_\  \   \ \  \ \  \\ \  \ 
   \ \____________\ \__\ \__\ \__\\ \__\ \__\\ \__\ \__\ \__\ \____________\   \ \__\ \__\\ \__\
    \|____________|\|__|\|__|\|__| \|__|\|__| \|__|\|__|\|__|\|____________|    \|__|\|__| \|__|
> Please give me your secret: 
> Here is your encrypted secret: iZzFsKme0oOdndOqgdnxsKmZ0KG/2o+hgdA=

```
The first secret seems to be a distraction, so the second one must be our flag. Both secrets are encrypted and we need to find how.
Luckily, there is no twist. Do some more reading and we find this python code:

```python
KEY = bytes.fromhex('deadbeef')
def encryptSecret(secret):
    lst_byte = []
    for i in range(len(secret)):
        enc_byte = ord(secret[i]) ^ KEY[i % len(KEY)]
        lst_byte.append(enc_byte.to_bytes(1, 'big'))
    
    return base64.b64encode(b''.join([_ for _ in lst_byte])).decode()
if __name__=='__main__':
    print(BANNER)
    secret = input("> Please give me your secret: ")
    print("\n> Here is your encrypted secret:", encryptSecret(secret))
```

So the flag is encrypted with the encryptSecret function:
- For each character in the input secret, it calculates the ASCII value and performs a bitwise XOR operation with a corresponding byte in the KEY.
- The result of this operation is converted to bytes format.
- These bytes are collected into a list, joined into a single bytes object, and then encoded into a base64 string.
- This base64 string is returned as the encrypted secret.

We have the XOR function, the KEY and the cyphertext, let's decrypt it. Here is how I did:
```python
import base64

def decryptSecret(encrypted, key):
    KEY = bytes.fromhex(key)
    encrypted = base64.b64decode(encrypted)
    lst_byte = []
    for i in range(len(encrypted)):
        dec_byte = encrypted[i] ^ KEY[i % len(KEY)]
        lst_byte.append(chr(dec_byte))
    
    return ''.join([_ for _ in lst_byte])

encrypted = 'iZzFsKme0oOdndOqgdnxsKmZ0KG/2o+hgdA='
key = 'deadbeef'
print(decryptSecret(encrypted, key))
```
The flag is: W1{_w3llC0mE_tO_w4nNaw1N_}
