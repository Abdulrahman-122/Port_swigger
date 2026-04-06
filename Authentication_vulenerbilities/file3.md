 Brute-forcing a stay-logged-in cookie
 This lab allows users to stay logged in even after they close their browser session. The cookie used to provide this functionality is vulnerable to brute-forcing.

To solve the lab, brute-force Carlos's cookie to gain access to his My account page.

    Your credentials: wiener:peter
    Victim's username: carlos 
    
  solution
  - first; login using wiener,peter
  
  extract the header id fromm using burp  -> delete userid=wiener(above)
  then delete the session of wiener value as we want to acess that one of carlos 
  then;
  make the payload over the stay-logged-in 
  then add the simple list
  note take that payload + decode it in the decoder in the burp using decoder base 64 
you will notice that
wiener:many nums
take these many nums and convert it to md5 on crack station 
it will be peter 
that means stay-logged-in is vulnerable to the username,password
now return to the intruder
go to payload processing
add ; 
hash then Md5
Add prefix: carlos
encode then Base64
as we want to build this in the attack

base-64(username:MD5(password))
ex;base-64(wiener:MD5(peter))
hit attack 
go to the highest response and render it's output 
you will see carlos is logged in and on update his email page 


