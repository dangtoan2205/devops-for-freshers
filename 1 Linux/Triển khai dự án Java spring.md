Triển khai dự án Java spring
------------
1. Cài đặt công cụ cần thiết => Công cụ
2. Xem và sửa file cấu hình => File cấu hình
3. Cài đặt và thiết lập database => Công cụ
4. Build dự án => Build
5. Run dự án => Run
6. Kiểm tra hoạt động => Check
-----------


## 1. Chuyển dự án lên server
Tới thư mục chứa tệp zip dự án

```
scp .\shoeshop-ecommerce.zip ubuntu@192.168.81.31:/home/ubuntu
```

## 2. Giải nén và chuyển dự án vào thư mục projects
```
unzip shoeshop-ecommerce.zip
```

```
sudo -i
```

```
cd /home/ubuntu/
mv shoeshop /projects/
```

## 3. Tạo user, quyền  cho dự án
```
adduser shoeshop
```

```
chown -R shoeshop. /projects/shoeshop/
chmod -R 750 /projects/shoeshop/
```
![image](https://github.com/user-attachments/assets/1c224692-fe72-4704-9393-e2780f8bd9ce)


## 4. Build dự án 

`Để có thể build dự án bằng Java ta có thể search từ khóa như sau: how to build java project`

- Ta sẽ cần sử dụng Java và Maven
- Ta cần có 1 file `pom.xml`



































































