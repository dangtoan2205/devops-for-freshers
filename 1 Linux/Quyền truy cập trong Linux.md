Quyền truy cập trong Linux
------------


## Tạo user mới trên hệ thống

```
sudo useradd manhnv1
```
hoặc

![image](https://github.com/user-attachments/assets/cc6338a6-5afb-4d6a-9a1f-729bec1f45e2)

```
sudo adduser manhnv2
```
![image](https://github.com/user-attachments/assets/b65db8c8-df61-4072-80f0-aa760f89256e)

![image](https://github.com/user-attachments/assets/625cc484-fe52-496e-a6f4-3c0250f5838d)


## Xem thông tin các user hiện có
```
cat /etc/passwd
```

## Xoá user 
![image](https://github.com/user-attachments/assets/49bcdb10-2e48-4d9c-a802-63bf29711d43)

```
sudo deluser manhnv1
```

## Tạo một Group
```
sudo groupadd devops1
```

## Xóa nột Group
```
sudo delgroup devops1
```

## Thêm một user vào Group
Cú pháp : `sudo usermod -aG <tên group> <tên user>`

```
sudo usermod -aG devops2 manhnv2
```

## Kiểm tra user trong group
![image](https://github.com/user-attachments/assets/caeaaf13-d78d-40b1-afe0-6890e2351298)
```
groups manhvn2
```

## Xóa một user ra khỏi group
![image](https://github.com/user-attachments/assets/fa59ae03-36b7-42a9-8fd9-2dd5121dddb5)

Cú pháp : `sudo deluser <tên user> <tên group>` 

```
sudo deluser manhnv2 devops2
```

-------------

Phân quyền
-----------
- Có 2 loại
    - Quyền truy cập
    - Quyền sở hữu
 
## Quyền sở hữu
![image](https://github.com/user-attachments/assets/c815bd5e-d686-4989-916c-377965fa9582)

<table border="1">
    <tr>
        <th>Permissions</th>
        <th>Links</th>
        <th>Owner (chủ sở hữu)</th>
        <th>Group (nhóm sở hữu)</th>
        <th>Size</th>
        <th>Month</th>
        <th>Day</th>
        <th>Time</th>
        <th>Name</th>
    </tr>
    <tr>
        <td>drwxr-xr-x</td>
        <td>2</td>
        <td>root</td>
        <td>root</td>
        <td>4096</td>
        <td>Jan</td>
        <td>7</td>
        <td>12:00</td>
        <td>datas</td>
    </tr>
</table>

## Đổi quyền chủ sở hữu hoặc nhóm sở hữu
Cú pháp: `sudo chown -R <chủ sở hữu>:<nhóm sở hữu>`

`-R : áp dụng quyền này cho tất cả các thư mục hoặc tệp thư mục con chứa trong thư mục cha`
```
sudo chown root:devops2  datas/
```
![image](https://github.com/user-attachments/assets/15f8a75a-4f5d-4d9f-9591-28fbe1355857)

## Giải thích chi tiết
rwx : 3 ký tự đại diện cho các quyền (đọc, ghi, thực thi)

![image](https://github.com/user-attachments/assets/67c9d2a2-4d7a-4254-b441-03302581d35d)

drwxrwxr-x  -> ugo (user | group | owner)

    - d : đại diện cho đây là thư mục. Nếu là - thì là tệp.
    - rwx : 3 ký tự tiếp theo đại diện cho các quyền (đọc, ghi, thực thi) -> đại diện cho các quyền của chủ sở hữu -> ở đây là root
    - rwx : 3 ký tự tiếp theo đại diện cho các quyền (đọc, ghi, thực thi) -> đại diện cho các quyền của nhóm sở hữu -> ở đây là devops2
    - r-w : 3 ký tự tiếp theo đại diện cho các quyền (đọc, ghi, thực thi) -> đại diện cho các quyền của người dùng khác trên hệ thống 

VD: Thêm quyền cho group
```
sudo chmod g=rwx datas
```

VD: Thêm nhiều quyền cùng một lúc

![image](https://github.com/user-attachments/assets/a4f1edff-2f01-409b-b36b-8d7e000031ab)
```
sudo chmod u=rwx,g=rx,o=- datas/
```

## Phân quyền bằng số 
`r=4, w=2, x=1 => 4+2+1=7`

Cú pháp: ` sudo chmod <quyền của chủ sở hữu><quyền của nhóm sở hữu><quyền khác> <file hoặc thư mục>`

VD: 
  - Ta muốn cho quyền của chủ sở hữu full quyền thì là 4+2+1 = 7
  - Ta muốn cho quyền của nhóm sở hữu chỉ có đọc và thực thi thì 4+1 = 5
  - Ta muốn nhóm người dùng khác không có quyền gì cả thì để 0

=> 
```
sudo chmod 750 datas/
```

![image](https://github.com/user-attachments/assets/928003fe-f081-4480-8da1-1cbe1777058f)




















