# Description:
Why search for the flag when I can make a bookmarklet to print it for me?
Browse [here](http://titan.picoctf.net:51073/), and find the flag!

# Solution:
Access the website, you will find the following javascript code:
```javascript
        javascript:(function() {
            var encryptedFlag = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓÓÇ¡¥Ìí";
            var key = "picoctf";
            var decryptedFlag = "";
            for (var i = 0; i < encryptedFlag.length; i++) {
                decryptedFlag += String.fromCharCode((encryptedFlag.charCodeAt(i) - key.charCodeAt(i % key.length) + 256) % 256);
            }
            alert(decryptedFlag);
        })();
```
Run with javascript compiler, flag:  picoCTF{p@g3_turn3r_0c0d211f}
