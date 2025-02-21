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
























