### The message processing interface of the wordpress plug-in WP Nano Ads has an XSS vulnerability.

**Official website**: https://cor.wordpress.org/plugins/wp-nano-ad/

**Version**：1.31

**Route**：/wp-admin/admin.php?page=wp_nano_ads_all_links&id=1&what=edit

**API function**: process messages

**Injection parameters**: blogrole_link

**Request method**：POST

#### 1.Vulnerability analysis

In the add_links.php file, the content passed by the user is directly inserted into the database for storage without any filtering. Attackers can construct malicious payloads to implement storage-type XSS attacks.

![image-20250313211049257](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250313211049257.png)

#### 2.Vulnerability verification and exploitation

Note that you need to select the appropriate version to successfully install the plug-in.

We construct the following payload:

```
test<script>alert(1)</script>
```

On the new or modified record interface page, change the "link url" parameter to payload. Then return to the Links main page to implement a storage-type XSS injection attack

![image-20250313211247802](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250313211247802.png)

![image-20250313211342643](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250313211342643.png)