Tư duy viết Dockerfile
--------

![image](https://github.com/user-attachments/assets/9fbb0233-657e-4c1a-946b-d756354a6d1a)



**FROM**
```
FROM node:20
```

-> Ta có base nodejs version 20

**WORKDIR**
```
WORKDIR/app
```

 -> Chỉ định thư mục làm việc

**COPY**

```
copy ..
```

-> Copy source code vào container

**RUN**
```
run pwd
```

-> Chạy câu lệnh

**ENV**

-> Khai báo các biến

**EXPOSE**
```
EXPOSE 8080:80
```

-> Port bên trái chính là port của server ứng với port bên phải là port của container

-> Định nghĩa ứng dụng trong container sẽ được chạy ở port nào

**CMD**


**ENTRYPOINT**


Viết Dockerfile tối ưu
-----------

![image](https://github.com/user-attachments/assets/b0261c22-6e2e-4edf-a438-8394352db537)


![image](https://github.com/user-attachments/assets/3b4dbc77-4feb-4e7f-a096-824c2678744c)


![image](https://github.com/user-attachments/assets/9f6d7dd6-2419-4e67-b4c3-1089971314bc)


![image](https://github.com/user-attachments/assets/94ded83c-7fd3-4dcf-822e-19831f349803)


















