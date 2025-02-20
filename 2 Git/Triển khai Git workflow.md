![image](https://github.com/user-attachments/assets/290b8220-26a4-4558-a994-9bf22c647d50)


## 1. Tạo nhánh mới
![image](https://github.com/user-attachments/assets/6c6cca3b-fb12-404c-958e-e88323462141)

- Tạo nhánh `develop` trong nhánh `main`

![image](https://github.com/user-attachments/assets/f592d489-e77e-4713-a4dc-a64d5b0eab64)

- Tạo nhánh `feature/frontend/login` trong nhánh `develop`

![image](https://github.com/user-attachments/assets/209fcdd9-8b21-4137-890e-ca16988c4d17)

## 2. Tạo ra 1 user mới

- Name: dev1
- Username: dev1
- Email: dev1@elroydevops.tech

-> Regular -> Create user

## 3. Thêm user  `dev1` vào dự án `shoeshop`

![image](https://github.com/user-attachments/assets/c096c9e7-9618-4d41-9f31-65f06e679576)

Chọn Project information -> Members -> Invite members 

![image](https://github.com/user-attachments/assets/550ebf75-1525-4410-99d4-3cfb2481269c)

## 4. Hợp nhất code vừa thêm được từ nhánh `feature/frontend/login` vào nhánh `develop`

Tại user `dev1` -> Thêm file login.txt

Để hợp nhất code cần tạo `merge request`

![image](https://github.com/user-attachments/assets/8eaa6106-9015-48b9-b490-c986ac2282e9)

Merge requests -> New merge request 

![image](https://github.com/user-attachments/assets/a774e0b6-9767-4e42-9f39-0fbaa6a4c87c)

Bên trái là chọn nhánh mà `dev1` vừa code 

![image](https://github.com/user-attachments/assets/37b00a75-e6a5-40d4-8157-a3311f07b994)

Bên trái là chọn nhánh muốn hợp nhất vào -> nhánh `develop` 

-> Compare branches and continue

![image](https://github.com/user-attachments/assets/c0533686-345c-4674-acdd-19badb55147d)

## 5. Cấu hình ngăn cho dev tự có quyền `approved` và `merge` vào nhánh develop
--------

Vào gitlab administrator

Chọn `Settings` -> `Repository` 

![image](https://github.com/user-attachments/assets/c9a6b466-2c64-4e23-a32b-b93be5f2981c)

Chọn `Default branch` -> `Expand` -> Chọn `develop`

![image](https://github.com/user-attachments/assets/bcdfda4a-224a-42fe-ba9c-b49d27e75d44)

-> Save changes

Tiếp theo Chọn `Protected branches` -> `Expand` -> Chọn `develop`

![image](https://github.com/user-attachments/assets/4c2813d5-0998-4fb0-a085-fde0213e9bd9)

Hiện tại ta có thể nhìn thấy 1 nhánh `main` đang được bảo vệ (lựa chọn mạc định)

![image](https://github.com/user-attachments/assets/1957dc30-4f5d-4550-8c57-4d471aa8ce2e)

Tiến hành chọn bảo vệ nhánh `develop`

- Branch: develop
- Allowed to merge: Maintainers (Chỉ có lead mới có quyền merge trực tiếp từ các nhánh feature vào nhánh develop)
- Allowed to push:
      - Maintainers  (Có thể commit trực tiếp trên nhánh đó)
      - No one  (Bất chứ khi mình làm cái gì hay chức năng gì đó đều phải cần gửi 1 request vào để lưu lại lịch sử chỉnh sửa)


![image](https://github.com/user-attachments/assets/233f5cee-f18c-4064-aff9-513c6f8d8427)


Admin

Approved -> Merge

![image](https://github.com/user-attachments/assets/3ddbee70-4dcb-44fa-b84d-eb873925d2ed)


Đối với vị trí Devops
---------------


Tag có ý nghĩa là đánh dấu mốc triển khai
----------

![image](https://github.com/user-attachments/assets/c7abacac-f2f4-4ce5-8462-2b963369140b)


Tag -> New tag 

![image](https://github.com/user-attachments/assets/a06855bb-5015-46e3-b3b3-6f058f2690d6)

Tạo ra tag như này tức là chúng ta đã đánh dấu phiên bản triển khai dự án.

VD: Tại dự án này ta đã đánh dấu release một chức năng nào đó




















