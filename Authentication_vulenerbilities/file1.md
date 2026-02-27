# what is the authentication?
  -it's way used to identify a user 
    - by using passwords, emails....
# authentications vs authrizations;
   - authentication;
     - identify the user through the password..
   - authrizations;
     - identify whether user is qualified to do something or not.
    
# how authentication vulunerability arises:
  -if the code is poor at it's implementations(broken authentications)
  - if the code is weak by  brute-force attacke .

# Vulnerability in password based authentication???
  -brute force attack
    - which means attacker trying to guess the password of the system more than one time.
    types;
      - Brute-forcing username;
        - sometimes using bruteforce you can build the name even the email you can build it.
      - Brute-forcing password;
        -if the password is easy to predict => brute force can find it => but with increasing the strength of it => brute force will be weak
        - so to solve this issue of weak brute force => use some prediction from your mind => like; if the password is password1? => and user change it it will be ;password! as the user doesn't need to change the whole of it to avoid forgot it .

# Username enumeration
  - now we want to enumerate the username from a list of guesses to see whether it's correct or not
  - enumeration done using brute force
  - but take care while the enumeration is running?
      - pay attention on these things;
        - status code => sometimes status code is changed if you enter special username or whatever => but in all cases it should be the same
        - Error messages => may  the error message change across the website => but in all cases it should be the same.
        - Response times => the time that response may change as you may enter a long password or whatever + but the time for check the credentials should be the same.
        -  This lab is vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:

    Candidate usernames
    Candidate passwords

To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page

analysis;
	- vulnerable ->  username,password .
	- username enumeration + password brute-force
	- candidate usernames,passwords
	
for enumeration the username;
add payload at the username then past the list of users inside payload conf
then run the attack
username=&admin&+&password=%3D+s6c3nlsx945pip8jgz0s
result;
Incorrect password for a payload its length bigger than the other ones
so this is the username; called; ad
for password brute-force.;
now make a payload at password; then paste the list of passwords into payload conf
then run the attack;
username=ad&password=password
focus on the length in the intruder 
result;
at the length choose the minimum length on my it was 184 = moon
username=ad&password=moon
now i will register for this and see.

------------
lab2; Username enumeration via subtly different responses 

 This lab is subtly vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:

    Candidate usernames
    Candidate passwords

To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page. 
analysis;
	- subtly vulnerable=>username(enumeration)+password brute-force 

output;

	to solve the attack ;
	username enumeration
	add the payload on the username
	paste the list of usernames
	then go to Greb extract
		add the  word that appears when you enter false username,password then => invalid username and passwords.
		then start  attack;
		after the attack finished filter this column invalid username... 
		to see if there's another row that has another value than invalid...
		you will see one once you see it
		grep it's username
		
username=as400&password=zu7mwvsfotbnin10qlpm
	password brute-force;
	after you put a payload on password + paste the list of password in it
	run the attack;
		username=as400&password=princess
		
things I learned from this lab 
you can outline a word in the code of the page and filter it by the attack to see if there's any attack that has another value than it or not
	if  there you make a subtle attack. and you can extract the name,password
     
------
lab3;
Lab: Username enumeration via response timing 
 This lab is vulnerable to username enumeration using its response times. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

    Your credentials: wiener:peter
    Candidate usernames
    Candidate passwords
hint;
To add to the challenge, the lab also implements a form of IP-based brute-force protection. However, this can be easily bypassed by manipulating HTTP request headers. 
analysis;
	- username enumeration(vulnerable) => focus on response times
x-Forwarded-For is used to generate an Ip for the request you sent to the application so you put payload on it to make it change the ip as the backend will block you for 30min if you used the same ip for another Get request 
username enumeration;
	X-Forwarded-For: 62
Username=anaheim&password=peterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeterpeter: 

password bruteforcing
to extract the password you make another payload 
like this
X-Forwarded-For: 62
Username=anaheim&password=peter: 
put payload around X-for... and make it's range from 101;200 to avoid bloking your request as you used in the first attempt from 0;100
also put payload around password 
but the answer with me is wrong so I will try later .
