# Description:
Do you know how to use the web inspector?
Start searching [here](http://titan.picoctf.net:58480/) to find the flag

# Solution:
Go to /about.html, inspect the page and you will find a strange string encoded to base64:
```html
<section class="about" notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMDdiOTFjNzl9">
```
Decode it and you will get the flag: picoCTF{web_succ3ssfully_d3c0ded_07b91c79}
