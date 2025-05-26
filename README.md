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
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database

Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/user-attachments/assets/744526bf-8602-4415-9179-82ec17b36629)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![image](https://github.com/user-attachments/assets/64d5bc61-d4c2-41c9-89ab-d64563745ea6)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/user-attachments/assets/8c30fb79-d26f-4ac1-8a90-e42fad71073b)

Click on the menu Login/Register and register for an account

![image](https://github.com/user-attachments/assets/8a71ffdb-aa94-4843-aea9-453088f8237f)

![Screenshot 2025-05-26 101047](https://github.com/user-attachments/assets/a31a6d89-fa41-4bcd-b27a-81ecfe739018)

![Screenshot 2025-05-26 101047](https://github.com/user-attachments/assets/3b5cc271-ab2f-477a-a4f4-b210ea966d9d)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/user-attachments/assets/299250e3-873b-4736-b0f8-4cdf2bea1f45)

![image](https://github.com/user-attachments/assets/b08342fe-b28a-4e8b-b904-6fde9413c7e4)

## Bypassing login field
The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![Screenshot 2025-05-26 101244](https://github.com/user-attachments/assets/03b3ce25-17ea-4e2c-b27e-acc41bc551b7)

![image](https://github.com/user-attachments/assets/c5553333-0959-41f4-8287-539ff322ee06)

## Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:
![Screenshot 2025-05-26 102003](https://github.com/user-attachments/assets/63253a1d-bf0e-4c13-8e2a-6b8641e0b1e7)

![Screenshot 2025-05-26 101244](https://github.com/user-attachments/assets/b5958e8e-d610-494d-ba9e-ae8fba0a1dee)

![image](https://github.com/user-attachments/assets/7d12a369-b5e9-4218-8580-c2b4a1055ce7)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.46.129/mutillidae/index.php?page=user-info.php&username=Ashqar%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

After adding the order by 6 into the existing url , the following error statement will be obtained:

![image](https://github.com/user-attachments/assets/626ee379-3955-470e-8a1f-fd8f47fb75a0)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/user-attachments/assets/f91f4bac-4b54-408d-8ff0-c7891e687eff)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5).

![image](https://github.com/user-attachments/assets/fe27607c-f149-4d3e-8407-393f12d9c4c2)


## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
