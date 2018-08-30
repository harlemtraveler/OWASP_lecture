# What are we going to Cover Tonight?
----
* What exercises you guys have tried?
* Which ones fail?
    * `CAPTCHA`
* Do any produce errors?
----

## **OWASP** Top 10 Run Through
----

1. **Injection** : untrusted data is sent to an interpreter as part of a command or query. 
2. **Broken Auth and Session Management** : Implemented incorrectly in development allows attackers to capture passwords, keys, tokens, or other flaws
3. **Cross Site Scripting**: applications include untrusted data into a new page without validation; allows browser based script execution
4. **Broken Access Control**: Restrictions of authenticated users arent enforeced. Users having `root` who shouldn't
5. **Security Misconfiguration** : secure settings need to be defined, implemented, and enforced; default settings are usually inherently vulnerable.
6. **Sensitive Data Exposure**: applications should encrypt senstive data, maintain offline backups, and restrict the amount of data they maintain.
7. **Insufficient Attack Protection**: patching is just as important as detecting, logging and responding. Validation and inputs are only the beginning
8. **Cross Site Request Forgery (CSRF)** : forces a logged on victim's browser to send a forged HTTP request, using the vitim's cookies. 
9. **Using components with known Vulns**: Sometimes the prettiest, or oldest technology isn't used for a reason.
10. **Underprotected APIs**: validate, validate, validate. Harden endpoints

----

## BurpSuite
----
1. Set up your proxy with your web browser ( for firefox it's under ```preferences>network>settings``` )
2. Start BurpSuit with the interceptor turned on/off depending on where you are in the process. 
----

## Cross-Site-Scripting
----

These attacks are injection attacks in which malicious scripts are injected into trusted sections of a website. XXS are performed using a web applications to send code in the form of a browser side script to a different end user. Anywhere user input is accepted is a vector for XSS. 

The end users browser has no way of knowing it should not run the code, because the script looks to be coming from a trusted source. The script can access any cookies, tokens, or sensitive information retained by the browser. Some scripts can rewrite entire HTML pages

### 3 Main Types of XSS Flaws:
1. **Stored XSS**: This is when user input it stored in a target server, and executed on retrieval. 
2. **Reflected XSS**: occurs when user input is immediately returned by the web application.
3. **DOM based XSS**: the entire attack surface is inthe DOM; data never leaves the browser

### Recommened CounterMeasures:
* Sanitize all input from users entering characters
* if you need to allow characters, encoded the data before renedered it in the browser
* use the `X-XSS-Protection` Response Header for built in protection

----

## Insecure Session Policy
----
* An attacker can take advantage of multiple sign-ins, or other flaws, to impersonate the session of a user. 

### Recommened CounterMeasures:
* Implement session timeouts
* Prevent opening more than one session

## Insecure Direct Object Referece:
* occurs when a developer exposes a referene to an internal implementation of an object; an attack will manipulate direct object referencs to access other properties of the object. 
* Attackers use parameter tampering to change references and violate the access control policy. This could lead to a **Path Traversal** attack or **Authorization Bypasses**


### Recommened CounterMeasures:
* have the server check a users priviledges before executing code
* servers should only provide user relevant information
* aviod direct object references when possible, use an index, an indirect reference map, or another indirect method. Only use the permissions auser has for that object. 
* Avoid exposing private object references ( primary keys, filenames, etc. )
* Validate any private object references with "accept known good"
* Verifiy authorization to all referenced objects 

## Cross Site Request Forgery
----

----

## XXE - Executed Eternal XML 
----

----

## RFD - Reflected File Download
----

----

## Path Traversal
----

----

## LFI/RFI 
----

----

## Bypassing Authencation
----

----

## Bypassing authorization
----

----

## Shell Uploading
----

----

## SQL Injection
----

----

**BWAPP** practive
