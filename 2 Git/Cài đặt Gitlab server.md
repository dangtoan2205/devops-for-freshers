## Tạo một server ubuntu tên là gitlab server

```
sudo -i
```

 Để cài 1 gitlab server ta cần thông tin như sau:

 Lên gg search: `gitlab ee packages`

 Link: 
 ```
 https://packages.gitlab.com/gitlab/gitlab-ee/packages/ubuntu/focal/gitlab-ee_15.11.13-ee.0_arm64.deb
```

![image](https://github.com/user-attachments/assets/a041d598-dbe2-4a50-ba9a-8f9ba0089470)

Tìm kiếm version sau:  `gitlab-ee_15.11.13-ee.0_arm64.deb`

-> Nên dùng bản version này

![image](https://github.com/user-attachments/assets/e51d5077-1306-499a-a544-8d167ca70a5c)

Chọn bản cài cho Ubuntu

![image](https://github.com/user-attachments/assets/0e4d5c5c-8802-4c71-a8fc-b15b38325091)

Click

Copy dòng này dán vào gitlab server

![image](https://github.com/user-attachments/assets/8f22cbbc-6b95-4905-aea8-a5e3c6015723)

```
curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
```

hoặc cài câu lệnh như sau nếu bị lỗi:

```
wget https://packages.gitlab.com/gitlab/gitlab-ee/packages/ubuntu/focal/gitlab-ee_15.11.13-ee.0_amd64.deb/download.deb -O gitlab-ee_15.11.13-ee.0_amd64.deb
```

Copy dòng tiếp theo để install gitlab ee

```
sudo apt-get install gitlab-ee=15.11.13-ee.0
```

Truy cập gitlab server bằng 1 domain
-----

```
nano /etc/hosts
```

![image](https://github.com/user-attachments/assets/bd7c4f4a-1ebd-4613-a25d-930560a8bf44)

Điền thông tin ip và domain của server vào (Đây là domain mình tự tạo)

```
192.168.80.106 git.elroydevops.tech
```

![image](https://github.com/user-attachments/assets/9a920366-ab9d-41c0-9a00-afb9f875aafe)


Tiếp theo tiến hành sửa url của git lab
----------

```
nano /etc/gitlab/gitlab.rb
```

![image](https://github.com/user-attachments/assets/7ebf98b4-aed1-4d5c-a2de-1bae0836385d)

Sửa thông tin của `external_url` thành

```
external_url 'http://git.elroydevops.tech'
```

```
gitlab-ctl reconfigure
```

Chờ quá trình cài đặt hoàn tất
-----

![image](https://github.com/user-attachments/assets/9ad4ee9e-54e8-4c45-89eb-695236c410b0)

Add host domain for window
------
Search gg : `host path windows`

```
c:\Windows\System32\Drivers\etc
```

![image](https://github.com/user-attachments/assets/7e8abd6d-f9d8-49a7-9c9b-bc1b44e03348)

![image](https://github.com/user-attachments/assets/83e6430a-4d9d-4d4d-b6b6-7be4218ccea6)

Thêm đoạn sau vào file `hosts`

```
192.168.80.106 git.elroydevops.tech
```

![image](https://github.com/user-attachments/assets/9457bf50-f170-4a6b-af9e-3f8279a65163)

Truy cập trên trình duyệt

`http://git.elroydevops.tech/`

![image](https://github.com/user-attachments/assets/c4e941b1-13e8-4dc6-a25c-b0982d978178)

Đăng nhập tài khoản vào gitlab server
---------

User name mặc định sẽ là: `root`

Để lấy password thì ta sẽ quay lại ssh server gitlab để lấy

```
cat /etc/gitlab/initial_root_password
```

![image](https://github.com/user-attachments/assets/f96095a3-6851-48c9-bc1a-32561d49ad8d)

![image](https://github.com/user-attachments/assets/5294d8f2-8034-4a82-bfbd-1682f5c60130)

Tiến hành copy password

`ItsGtF3vwYcbt4Gq9ABBTEiR21Zs8SVWg3IiOMKfJlw=`


Ta sẽ cần disable việc tạo mới user
---------

Truy cập đường link
```
http://git.elroydevops.tech/admin/application_settings/general#js-signup-settings
```

Nên bỏ tích 2 dòng này đi. Khi tạo mới user mình sẽ không cho hị tự tạo user mà mình sẽ tạo cho họ

![image](https://github.com/user-attachments/assets/cff376fe-5af4-4950-b69a-29dca8d478fe)

![image](https://github.com/user-attachments/assets/50686b81-e54b-40ab-802b-76e05c0cfc25)

![image](https://github.com/user-attachments/assets/b6a66f04-7a23-4f2e-b06c-21e88bbfefe4)

Lưu thay đổi


Tiếp theo sẽ đến phần CI/CD

![image](https://github.com/user-attachments/assets/88c629fc-1f57-46d1-ae61-9c0bf0daaaf1)

Chọn dòng này và bấm `Expand`

![image](https://github.com/user-attachments/assets/4c2f769f-d575-4a49-bb52-c5364b804a9d)

![image](https://github.com/user-attachments/assets/66d7624c-848a-4eef-be19-1a380b511aaf)

Bỏ tùy chọn này đi

![image](https://github.com/user-attachments/assets/d9d09737-9dea-49dc-afcc-03ca2f6f3255)

Lưu thay đổi

Tiến hành đổi mật khẩu của user root
----

![image](https://github.com/user-attachments/assets/348e3011-3893-4f86-871f-7336fe27a071)

Chọn Edit profile -> Password

![image](https://github.com/user-attachments/assets/1a6e2763-e838-4c96-8a6c-bbc3479a80a7)


Giới thiệu qua về gitlab
---------

1/ Tạo user mới

![image](https://github.com/user-attachments/assets/74945c61-7bc2-41f5-994d-66bfc11e6178)

Admin -> New user

![image](https://github.com/user-attachments/assets/3e7cf872-7142-47de-8dc9-5ce0bd9b0a0b)

Chọn quyền

![image](https://github.com/user-attachments/assets/cfac03ee-e178-4adc-b5d8-a32778895f43)

Nếu là user bình thường thì chọn -> Regular
Nếu là admin thì chọn -> Administrator

-> Create user

Để tạo mật khẩu ta phải nhấn vào `Edit`

![image](https://github.com/user-attachments/assets/ded647ac-6955-4a83-b42c-69c869556c2b)

![image](https://github.com/user-attachments/assets/812d095e-fe83-4244-acd0-04e9b456a741)


Chú ý: Mỗi khi mình muốn sử dụng gitlab server này thì trên mỗi máy tính mình đều phải cần add hosts

```
c:\Windows\System32\Drivers\etc
```

```
192.168.80.106 git.elroydevops.tech
```

![image](https://github.com/user-attachments/assets/bbdd22aa-2caa-43d9-a1ce-49b74fa01e86)


**Bật dịch vụ GitLab khi khởi động**

```
systemctl enable gitlab-runsvdir
```

```
systemctl is-enabled gitlab-runsvdir
```

- Nếu kết quả là `disabled`, bạn cần bật dịch vụ.
- Nếu là `enabled`, GitLab đã được cấu hình tự động chạy.












