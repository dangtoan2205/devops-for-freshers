Triển khai các dự án Frontend
-----------

Để triển khai dự án cần 3 phần:
  - Dự án nào thì có công cụ tương ứng
  - Chỉ cần lưu ý duy nhất là file cấu hình trong dự án
  - Triển khai dự án chỉ cần 2 bước: 1 là build và 2 là run

Chú ý:
  - Mỗi dự án sẽ có thư mục riêng
  - Mỗi dự án sẽ có user riêng


## Câu lệnh copy dự án từ máy cá nhân chuyển lên server
1/ Vào tệp chứa thư mục dự án </br>
2/ Chuột phải chọn Termainal </br>
3/ paste câu lệnh thực thi sau: </br>

![image](https://github.com/user-attachments/assets/1fd5ab7b-e6a2-417d-9cf6-b4e082808648)

Giải thích: 
`scp <tên thư mục chứa dự án> <tên user đăng nhập trên ubuntu server>@<địa chỉ ip của ubuntu server>:<vị trí lưu tệp trên ubuntu server>`

```
scp todolist.zip manhnv@192.168.1.110:/home/ubuntu
```

## Cài công cụ unzip
```
sudo -I
apt install unzip
```

```
cd /home/ubuntu
unzip todolist.zip
```

---------
Bắt đầu
---------

## 1 Tạo thư mục mới và di chuyển project vào thư mục vừa tạo
```
mkdir /projects
mv todolist /projects/
ls -l /projects/
```

## 2 Thêm user riêng của dự án
![image](https://github.com/user-attachments/assets/dae376a6-79b2-4c99-aa73-3168b6cc36f5)

```
adduser todolist
```

## 3 Thay đổi chủ sở hữu sang user todolist
![image](https://github.com/user-attachments/assets/2f6e105d-f13f-43a9-b035-7e970d661673)
```
chown -R todolist:todolist /projects/todolist/
```

![image](https://github.com/user-attachments/assets/5291d88d-4eb0-4b79-9699-7f2c2810220d)

Chỉnh sửa lại quyền thư mục

```
chmod -R 750 /projects/todolist
```


## Build and Run
```
apt update
apt install nodejs -y
apt install npm -y
```

## 4 Chuyển đổi user
```
su todolist
```

## 5 Cài đặt npm
```
npm install
npm run build
```

```
exit
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
apt-get install -y nodejs
```

```
su todolist
npm run build
```
![image](https://github.com/user-attachments/assets/34566445-3c5f-48e1-ac75-1c99c61e1944)

Như vậy nó đã build cho mình 1 thư mục tên là dist

## Run dự án

```
npm run serve
```

-------------
6 Run dự án bằng Nginx
------------


```
exit
apt install nginx -y
```

Mặc định khi cài Nginx sẽ chạy trên cổng 80.

Kiểm tra
```
netstat -tlpun
```

## Cấu hình Nginx

```
cd
cd /etc/nginx
nano conf.d/todolist.conf
```

```
server {
  listen 8081;
  root /projects/todolist/dist/;
  index index.html;
  try_files $uri $uri/ /index.html;
}
```

```
nginx -t
```

```
systemctl restart nginx
```

## Fix lỗi
![image](https://github.com/user-attachments/assets/aec8aa84-1b80-41f1-8b58-1ea201bde764)


![image](https://github.com/user-attachments/assets/fe9fbaf0-7b6f-4bd4-8554-05c99ab0fb13)

Ta thấy những người dùng khác sẽ không có quyền truy cập đến folder todolist này.

-> Mỗi một công cụ đều sẽ có 1 user riêng của nó. Nginx cũng không ngoại lệ.

Mở file user
```
cat /etc/passwd
```
![image](https://github.com/user-attachments/assets/f3081db8-42bd-4a4c-b70c-5dadcb24c7a1)

Ta mới tạo thêm 1 user là todolist

-> Kiểm tra user của Nginx là user gì
```
cat /etc/nginx/nginx.conf
```

![image](https://github.com/user-attachments/assets/a4a92fe5-3985-455f-979b-984a90a63d57)

Như vậy ta thấy Nginx đang sử dụng user là www-data

-> Để dự án của mình chạy được trên nginx tức là user của nginx tối thiểu phải nằm trong group todolist

Thêm www-data vào group todolist 
```
usermod -aG todolist www-data
systemctl restart nginx
```

### Có thể sử dụng câu lệnh thay thế 

systemctl restart nginx <=> nginx -s reload


------------------

Triển khai dự án bằng ReactJS
------------------

## 1/ Di chuyển dự án lên server
```
scp .\vision.zip ubuntu@192.168.81.95:/home/ubuntu
```

```
sudo -i
cd /home/ubuntu
unzip vision.zip
mv vision /projects/
```

## 2/ Tạo user mới
```
adduser vision
```

## 3/ Thay đổi chủ sở hữu và quyền tác động của folder vision
```
chown -R vision. /projects/vision/
chmod -R 750 /projects/vision/
```

## 4/ Chuyển qua tài khoản mới tạo
```
cd /projects/vision
ls
su vision
```

## 5/ Cài công cụ npm cho project
```
npm install
```

## 6/ Triển khai dự án (Tạo ra 1 service để chạy dự án này)
```
exit
nano /lib/systemd/system/vision.service
```

```
[Service]
Type=simple
User=vision
Restart=on-failure
WorkingDirectory=/projects/vision/
ExecStart=npm run start -- --port=3000
```

```
systemctl daemon-reload
systemctl start vision
systemctl status vision
```















