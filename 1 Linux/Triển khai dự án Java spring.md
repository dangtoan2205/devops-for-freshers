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

`Để có thể build dự án bằng Java ta có thể search từ khóa như sau: how to build java project` -> `how to install java on ubuntu`

- Ta sẽ cần sử dụng Java và Maven
- Ta cần có 1 file `pom.xml`

## Cài đặt

`Chú ý: verion của công cụ phải lớn hơn hoặc bằng version của dự án`

VD trong dự án có file cấu hình lưu ở `pom.xml`

![image](https://github.com/user-attachments/assets/c6462071-71c1-43b1-91ea-53d1bd1ede38)

Như vậy dự án yêu cầu java version 1.8

```
apt install openjdk-17-jdk openjdk-17-jre
```

Cài maven

```
apt install maven
```

Ta thấy trong dự án có cả database -> search: ` Config database spring boot`

Ta sẽ cần 1 file là `application.properties`

![image](https://github.com/user-attachments/assets/daccce4e-e9cb-46ef-a06e-55f889598905)


![image](https://github.com/user-attachments/assets/5afc028d-e07e-4b95-9d89-1ac419577ec9)

Ta sẽ cần các thông tin của DB như sau:
- address_server
- port
- database_name
- username
- password

-> Tiến hành cài đặt Maria DB

```
apt install mariadb-server
```

Kiểm tra
----
![image](https://github.com/user-attachments/assets/8365ca82-0001-47ec-90c1-fe2bb41e9605)

Như vậy ta thấy địa chỉ 127.0.0.0:3306 này chỉ chạy trên local (tức là chỉ truy cập được trên ubuntu của mình)

Để được từ bên ngoài có thể truy cạp vào địa chỉ ip DB của mình thì mình cần cấu hình public ra bên ngoài

Các bước thực hiện:

```
systemctl stop mariadb
```

```
ls /etc/mysql/mariadb.conf.d/
```

```
nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
![image](https://github.com/user-attachments/assets/db32016c-68b4-4dd3-ad6b-9e73c9cf1e61)

![image](https://github.com/user-attachments/assets/25621dd6-76f3-4572-9c53-098c07f5181d)

Tìm đến dòng `bind-address`

Và sửa `127.0.0.1` thành `0.0.0.0`

```
systemctl restart mariadb
```

Sau đó kiểm tra lại DB đã được public hay chưa
```
netstat -tplun
```

![image](https://github.com/user-attachments/assets/6e48bd50-431e-46eb-a36b-da6e9e74771a)

Ta thấy 0.0.0.0:3306 đã được public ra bên ngoài

-> Có thể cài đặt server db riêng ra 

Thêm file `shoe_shopdb.sql` vào trong maria db
----

```
mysql -u root
```

```
show databases;
```

```
create database shoeshop;
create user 'shoeshop'@'%' identified by 'shoeshop';
grant all privileges on shoeshop.* to 'shoeshop'@'%';
flush privileges;
```

Chú giải:

- `create user 'user name'@'phạm vi'  identified by 'mật khẩu';`

-> % là có thẻ truy cập được đến tất cả các server

- gán quyền cho user này tác động đến nhưng đối tượng nào:

`grant all privileges on (user name).(áp dụng ở đâu) to '(user name)';`

-> * là trên tất cả các tài nguyên

- flush privileges;

-> Lưu lại các quyền

Tiến hành đăng nhập bằng user `shoeshop` mà mình vừa tạo

```
exit
```

```
mysql -h 192.168.81.31 -P 3306 -u shoeshop -p
```

Chú giải:

- h : viết tắt của từ host là Ip server của mình
- P : chỉ định port của mysql
- u : user
- p : mật khẩu

kiểm tra database đã có hay chưa
```
show databases;
```

Tiến hành imporrt database `shoe_shopdb.sql` vào để sử dụng.
-----
```
use shoeshop;
source /projects/shoeshop/shoe_shopdb.sql
```

## 5. Sửa file cấu hình database

```
nano /projects/shoeshop/src/main/resources/application.properties
```

![image](https://github.com/user-attachments/assets/ca8d42b1-da54-4f78-a5a6-36910b26738b)


## 6. Build
```
cd /projects/shoeshop/
mvn install -DskipTests=true
```

- -DskipTests=true : bỏ qua bước test của tool maven

![image](https://github.com/user-attachments/assets/75a2a545-8961-490c-b967-2106f23fcbe6)

Như vậy đã build dự án thành công được lưu ở thư mục `target`

![image](https://github.com/user-attachments/assets/d8ea2d17-b145-468e-ac46-04b8b081f386)

File build là file .jar

## 7. Run 
```
java -jar target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar
```

![image](https://github.com/user-attachments/assets/3221528c-f5e1-44b1-a0b0-7adf03b78bbe)

Dự án đã được chạy thành công với port 8080.


Câu lệnh chạy dưới nền
```
nohup java -jar target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar 2>&1 &
```

Kiểm tra tiến trình dự án đang chạy

```
ps -ef| grep shoe
```

Dừng dự án đang chạy

```
kill -9 56552
```


-9 là sẽ buộc dừng
-56552 là piID


## 8. Chạy dự án với user đúng của dựa án

```
su shoeshop
```

```
ls -l
```

Xóa tệp và thư mục vừa được build dưới quyền root đi và sẽ run dưới user shoeshop
![image](https://github.com/user-attachments/assets/1099c06b-fc6c-4e35-88f4-98134f626c59)

```
exit
```

```
rm -rf nohup.out target/
```

```
su shoeshop
```

Run dự án

```
mvn install -DskipTest=true
```

```
nohup java -jar target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar 2>&1 &
```


















