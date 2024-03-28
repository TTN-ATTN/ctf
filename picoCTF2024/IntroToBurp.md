# Description:
Additional details will be available after launching your challenge instance.

# Solution:
Access the website, I was asked to fill in a form.

After fill all the information, I was asked to fill in an OTP for 2fa authentication.

I tried to look for more information, so I first accessed the cookie.
```
session = .eJxNjE0OwiAQhe_C2kVh2gG8DBlhiMYWCJQ0xnh3cdO4e-97P2_hH_tLXAURiYvwrUa35yengWxQRukISAH1ZKVBM3tcpFULAkXWgHZCjGMX-7q6RBufT3kvQ0sLMKthC7V25BrOvNxzYpf6duP6KyoYsDeufy-fLzMyLl0.ZgUE5Q.jJJbBVGemZ1b6_teOvUTgeGAtvQ
```
Since this is a flask session sookie, I tried decoding it to see if there is something interesting.

Decoded result:
```
{
    "city": "aaa",
    "csrf_token": "9d2827f36ad670918684c65192563afe7369066f",
    "full_name": "aaa",
    "otp": "193342",
    "password": "aaa",
    "phone_number": "123",
    "username": "aaa"
}
```

Found the OTP, I entered it and got the flag:  picoCTF{#0TP_Bypvss_SuCc3$S_c94b61ac}
