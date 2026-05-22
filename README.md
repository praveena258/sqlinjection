# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 
Identify IP address using ifconfig in Metasploitable2
#OUTPUT
<img width="686" height="397" alt="image" src="https://github.com/user-attachments/assets/e9bf21f7-dcdf-4d49-9af7-22fbf905e256" />
Command: ifconfig

Explanation (simple):

ifconfig = interface configuration
This command shows the network settings of the machine.
It displays details like:
IP address (the machine’s address on the network)
Subnet mask
MAC address
Network interface names like eth0



Select Multidae from the menu listed as shown above. The page is displayed as below:
##  OUTPUT

<img width="820" height="830" alt="image" src="https://github.com/user-attachments/assets/88d91c0f-b118-4f26-8050-4b1c6de8671d" />


Click on the menu Login/Register and register for an account
##  OUTPUT

<img width="827" height="897" alt="Screenshot 2026-05-21 203302" src="https://github.com/user-attachments/assets/f5bf5208-f32e-4a58-9f5e-a3ef78549ae2" />


Click on the link “Please register here”
##  OUTPUT


<img width="684" height="630" alt="Screenshot 2026-05-21 205122" src="https://github.com/user-attachments/assets/31fed072-9c8e-4d3e-9421-5c6ec715770f" />




The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:


($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
##  OUTPUT


<img width="916" height="867" alt="Screenshot 2026-05-21 204253" src="https://github.com/user-attachments/assets/71f1277a-2a87-4189-ab3f-fa2fd2cab1fd" />

Click “Login”. The logged in page will show as below:
##  OUTPUT
<img width="942" height="866" alt="Screenshot 2026-05-21 204400" src="https://github.com/user-attachments/assets/4ea23f92-d073-415d-b6f6-93c2304a2e8f" />


If error faced in registration follow the following steps in metasploitable 2:


This issue is caused by a misconfiguration in the config.inc located in the /var/www/mutillidae folder on Metasploitable 2 VM.



# Test the new configuration
Alright. Now is time to test if we managed to fix the database issue. Go ahead and register a new account on the Mutillidae webpage.

 The Mutillidae database error no longer appears 
#OUTPUT



Now after logging out you will see the login page. In the login page give ganesh’ # (myusername). You can see the page now enters into the administrator page as before when giving the password.
#OUTPUT


Click the login button and you will see it enter into the administrator page.
#OUTPUT

<img width="684" height="630" alt="Screenshot 2026-05-21 205122" src="https://github.com/user-attachments/assets/96e5dd7f-9a87-4eeb-94d8-b66609f9fd01" />


## Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:
##  OUTPUT


<img width="1020" height="800" alt="image" src="https://github.com/user-attachments/assets/57da6eca-a94b-44a7-84b3-43f046f7bd2b" />

<img width="994" height="518" alt="image" src="https://github.com/user-attachments/assets/ca2cf4a9-a5bd-47ad-ac00-c51de6dcc88d" />


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
##  OUTPUT

<img width="935" height="146" alt="image" src="https://github.com/user-attachments/assets/f7663511-af83-41f8-9612-1e2edda8b5ec" />


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:
##  OUTPUT



<img width="712" height="274" alt="image" src="https://github.com/user-attachments/assets/a0763b8c-ed41-4ce3-967e-13f5c5658566" />

After adding the order by 6 into the existing url , the following error statement will be obtained:
##  OUTPUT


<img width="744" height="291" alt="image" src="https://github.com/user-attachments/assets/5394dcc5-e4ef-4cf2-946d-6d5aea09fa3a" />


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
#OUTPUT

<img width="727" height="251" alt="image" src="https://github.com/user-attachments/assets/ffaa8e88-cc57-4ad0-904e-bbd2adad3408" />



 As it is having 5 columns the query worked fine and it provides the correct result
##  OUTPUT




Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5).
##  OUTPUT
<img width="684" height="630" alt="Screenshot 2026-05-21 205122" src="https://github.com/user-attachments/assets/9c9a9e65-c2ef-4b34-b0b4-cb40847f7c61" />



As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
##  OUTPUT

<img width="802" height="279" alt="image" src="https://github.com/user-attachments/assets/9bb735fd-79e6-41cf-95e6-9c4009db8121" />





Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.
##  OUTPUT

<img width="684" height="630" alt="Screenshot 2026-05-21 205122" src="https://github.com/user-attachments/assets/9c9a9e65-c2ef-4b34-b0b4-cb40847f7c61" />

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’
##  OUTPUT

<img width="712" height="254" alt="image" src="https://github.com/user-attachments/assets/00d8d828-f43d-4df8-9f35-40a31407cc76" />



The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
##  OUTPUT


<img width="885" height="577" alt="image" src="https://github.com/user-attachments/assets/b5bcd360-dffb-43c1-895e-b1cf6ff8ad22" />

The column names of the accounts is displayed below for the following url:


Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).
##  OUTPUT

<img width="912" height="432" alt="image" src="https://github.com/user-attachments/assets/8931a208-83a9-4b5d-83ad-fe512a96477b" />


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).


##  OUTPUT
<img width="922" height="614" alt="image" src="https://github.com/user-attachments/assets/bc928612-0210-4ae7-ac56-d143381b7bec" />



## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
