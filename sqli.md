The source point is located at line 100 of `youkefu-master\src\main\java\com\ukefu\webim\web\handler\admin\system\MetadataController.java`, where the `addsqlsave` method has a parameter named `datasql` of type `String`. The request path for this method is defined on line 98 as `/addsqlsave`.

<img width="719" alt="image" src="https://github.com/user-attachments/assets/4f365307-cb62-4455-8bf6-81654affd497" />
The sink point is at line 183 of `youkefu-master\src\main\java\com\ukefu\util\metadata\UKDatabaseMetadata.java`, in the `statement.executeQuery` method.

<img width="725" alt="image" src="https://github.com/user-attachments/assets/8e2831be-85c2-410d-b319-48d6f7e509ae" />


At line 137 of `youkefu-master\src\main\java\com\ukefu\webim\web\handler\admin\system\MetadataController.java`, it is indicated that the `addsqlsave` method, where the source point is located, redirects to the `/admin/metadata/index.html` page after execution. This is highly likely to be the functional module page where the vulnerability exists.
<img width="730" alt="image" src="https://github.com/user-attachments/assets/81eada29-881e-4fd2-976c-93684b91e51c" />

Start the application, log in to the Youkefu system as an administrator, then access `http://ip:port/admin/metadata/index.html` in the browser. This page corresponds to the system's metadata management feature, and it is presumed that the specific functionality is the "Import SQL" feature.
<img width="705" alt="image" src="https://github.com/user-attachments/assets/6e2aee2e-2797-4d67-b382-aaf290238469" />

<img width="710" alt="image" src="https://github.com/user-attachments/assets/97af1b0d-07de-4c86-a059-fa95455176fa" />

The SQL injection vulnerability was triggered through the URL captured using BurpSuite:  
```
http://ip:port/admin/metadata/addsqlsave
```
http://192.168.96.137:8088/admin/metadata/addsqlsave.html?name=sleep&data
sql=select+sleep(10);

After sending the request, it took 10275ms to return a response, indicating that the SQL query was successfully executed. This confirms the presence of an SQL injection vulnerability.
