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
Account locking
server uses this mechanism to defence for any attack that attacker want to do by locking websites -> attacker can't do any brute-force attack
so ; you need to make alist of usernames to make  enumerations along with the passwords (each username has a different password in order to avoid any blocking + make sure the number of attempts you have been allowed to sent to the server 
also make sure the concurrent requests that are send to the server are; 1 at a time

if the way of enumeration list of username,passwords didn't work;
->use; Credential stuffing attacks 
	you pass a composed of username:password pairs inside the file you will pass to the login (as many people uses the same passwords on many websites)
Lab: Username enumeration via account lock

This lab is vulnerable to username enumeration. It uses account locking, but this contains a logic flaw. To solve the lab, enumerate a valid username, brute-force this user's password,then access their account page. 

steps to solve this lab;
	- use invalid username to login in in order to open the page on burp
	- use intruder
	- use Cluster bomb attack and add payload to the username like this
		username=§invalid-username§&password=example§§
		then add another payload to the end of the line
		for username; add list of usernames for username
		for second payload -> add null payload 
		go to options to send payload 5 times -> then start the attack notice; the aim of null payload is to repeat the request 5 times or whatever in order to see the error message that you want
	- note ; username with error message; You have made too many incorrect login attempts (it's response time is longer is was expected)
	-  do sniper attack on the password then add the list of passwords then add the username that you was selected .
	- after adding the list of passwords to the password -> add grep for the error message that you selected before
	- after attack has been done -> look at the response that didn't contain any error messages(pull it's passowrd) 
	- wait a minute in order to allow web to reset then login using the password , username that you identified.
	note; 
	for the first attack -> we need to trigger the server in order to see it's different behaviour as the server won't tell you that username is correct untill you reach the amount of attempts 
	by seeing this error message;ou have made too many incorrect login attempts in this case you knew that this username is correct
	so go to secnond attack
	- why we add second payload in the first attack (null payload) in order to repeat the username the max amount of attempts to see the error message that we want to expect.
	
	
now let's work on the lab;

list of the usernames;
carlos
root
admin
test
guest
info
adm
mysql
user
administrator
oracle
ftp
pi
puppet
ansible
ec2-user
vagrant
azureuser
academico
acceso
access
accounting
accounts
acid
activestat
ad
adam
adkit
admin
administracion
administrador
administrator
administrators
admins
ads
adserver
adsl
ae
af
affiliate
affiliates
afiliados
ag
agenda
agent
ai
aix
ajax
ak
akamai
al
alabama
alaska
albuquerque
alerts
alpha
alterwind
am
amarillo
americas
an
anaheim
analyzer
announce
announcements
antivirus
ao
ap
apache
apollo
app
app01
app1
apple
application
applications
apps
appserver
aq
ar
archie
arcsight
argentina
arizona
arkansas
arlington
as
as400
asia
asterix
at
athena
atlanta
atlas
att
au
auction
austin
auth
auto
autodiscover


for the list of the passwords

123456
password
12345678
qwerty
123456789
12345
1234
111111
1234567
dragon
123123
baseball
abc123
football
monkey
letmein
shadow
master
666666
qwertyuiop
123321
mustang
1234567890
michael
654321
superman
1qaz2wsx
7777777
121212
000000
qazwsx
123qwe
killer
trustno1
jordan
jennifer
zxcvbnm
asdfgh
hunter
buster
soccer
harley
batman
andrew
tigger
sunshine
iloveyou
2000
charlie
robert
thomas
hockey
ranger
daniel
starwars
klaster
112233
george
computer
michelle
jessica
pepper
1111
zxcvbn
555555
11111111
131313
freedom
777777
pass
maggie
159753
aaaaaa
ginger
princess
joshua
cheese
amanda
summer
love
ashley
nicole
chelsea
biteme
matthew
access
yankees
987654321
dallas
austin
thunder
taylor
matrix
mobilemail
mom
monitor
monitoring
montana
moon
moscow
solution;username=alpha&password=abc123
as you see ; when access the http response -> you will see my account instead of invalide username or password 

------

User rate limiting;
	-> A way websites are using to block the IP of the hacker who can login multiple times at a time
	
	-> to unblock the the IP address you need to ;
		- wait after being blocked by an elapsed time
		- Manually by the admin
		- Manually by the user who complete a captcha(a problem you solve to ensure  that you aren't a hacker.)
		
	=> but all of these aren't secure at all as you can bypass your block as the lab we did before
	-> you can gues what number of passwords to bypass to the server within a single request.

now;
authentication -> check the identity of the person who want to login in 
autherization -> grant the resouces , data to the user who is being authenticated into the website.
so : once user open the page -> the server send a token to him contain it's name,password into base64 like that and store it into autherization  header of the page;
	Authorization: Basic base64(username:password) 
	note; this returning from the server to the user is not secure as any one can vulnerable that autherization and use it in more contexts
	now;
Vulnerabilities in multi-factor authentications?
	- multi-factor authentications -> website uses more than way to check the identity of the user 
		while single factor authentication -> website uses one way to check the identity (writing his password)

		- however -> using single factor authentication is not secure while using 2FA is secure one especially if it was dependant on multiple factor not the same one like; using password to login with another code depend on email credentials.
			-2FA -> depend on physical device to generate the token from it (in this case it's secure)
				-while some websites send SIM tokens to the users (but in this case ; attacker can fraudulrnltly user and use it's sim card with it's number and then receive all SMS messages that sent to him in this case he can intercept these message and hack him
Now How to bypass two factor Authentications?
	- once user start to logged in between entering the password,entering the varification code -> you mush test to see if you can skip to logged into page 
	
let's solve this lab to understand more;

This lab's two-factor authentication can be bypassed. You have already obtained a valid username and password, but do not have access to the user's 2FA verification code. To solve the lab, access Carlos's account page.
    
    Your credentials: wiener:peter
    Victim's credentials carlos:montoya
	
solution ;

	- log in as your self
	- click email client on the top of the page
	- note the end of the url of your varification code after  you take the code from the email page you will put it into varification code then note the end of the url as -> my account paste that end
	- do the same with the victim info
	- then you will access to the victim varification code after 1st step of 2FA then at the end of that page write instead of login -> myaccount the lab will be solved directly.
	




