### The message processing interface of the wordpress plug-in Simple Amazon Affiliate has an XSS vulnerability.

**Official website**: https://wordpress.org/plugins/simple-amazon-affiliate/

**Version**：1.0.9

**Route**：/wordpress/wp-admin/admin.php?page=saa-affiliate

**API function**: process messages

**Injection parameters**: msg

**Request method**：GET

**Complete vulnerability trigger related interfaces and parameters**：/wordpress/wp-admin/admin.php?page=saa-affiliate&msg=%3Cscript%3Ealert%281%29%3C%2Fscript%3E

#### 1.Vulnerability analysis

In the simple-amaon-affilliate.php file, there is an interface that can pass in the parameter msg, and it will be directly output in the page in the form of echo. It has not been subjected to any security filtering verification, resulting in an XSS vulnerability.

![image-20250302212559402](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250302212559402.png)

#### 2.Vulnerability verification and exploitation

We construct the following URL:

```
http://localhost/wordpress/wp-admin/admin.php?page=saa-affiliate&msg=%3Cscript%3Ealert%281%29%3C%2Fscript%3E
```

After accessing, it is found that the page pops up successfully, and there is an XSS vulnerability.

![image-20250302212822630](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250302212822630.png)