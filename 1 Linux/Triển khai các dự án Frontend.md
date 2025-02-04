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
scp todolist.zip manhnv@192.168.1.110:/home/manhnv
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

## Tạo thư mục mới và di chuyển project vào thư mục vừa tạo
```
mkdir /projects
mv todolist /projects/
ls -l /projects/
```

## Thêm user riêng của dự án
![image](https://github.com/user-attachments/assets/dae376a6-79b2-4c99-aa73-3168b6cc36f5)

```
adduser todolist
```

## Thay đổi chủ sở hữu sang user todolist
![image](https://github.com/user-attachments/assets/2f6e105d-f13f-43a9-b035-7e970d661673)
```
chown -R todolist:todolist /projects/todolist/
```

![image](https://github.com/user-attachments/assets/5291d88d-4eb0-4b79-9699-7f2c2810220d)

Chỉnh sửa lại quyền thư mục

```
chmod -R 750 /projects/todolist
```


## 













