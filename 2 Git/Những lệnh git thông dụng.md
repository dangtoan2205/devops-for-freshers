Với mỗi dự án ta nên tạo một group để chứa những service của dự án đó
-------

```
toandn
Administrator123a@
```

## 1. Tạo group 

-> Create a group

- Group name: shoeshop
- Visibility level: Private
- Role: Devops Engineer

-> Just me

![image](https://github.com/user-attachments/assets/336f2a8b-efa4-46d1-b40f-4e140a60b03b)

## 2. Tiến hành tạo project trong group

![image](https://github.com/user-attachments/assets/a09d703b-3104-4e54-b7f0-069afef007c2)

-> New project -> Create blank project

- Project name: shoeshop

![image](https://github.com/user-attachments/assets/7a53cbb6-9e9a-4fb1-9448-15c35f08f2f2)

## 3. Clone source code về ubuntu lab

```
mkdir /data
cd /data
```

![image](https://github.com/user-attachments/assets/f19b8eb7-1f6d-4f52-aa42-5c879af76878)

```
git config --global user.name "Dang Ngoc Toan"
git config --global user.email "toan.dang@elroydevops.com"
```

Kéo dự án trên gitlab về

![image](https://github.com/user-attachments/assets/de357c77-70c6-4927-aa55-9158a2334941)

```
git clone http://git.elroydevops.tech/shoeshop/shoeshop.git
```

Nhập tài khoản mật khẩu:

![image](https://github.com/user-attachments/assets/0ab30979-899a-40a0-b62e-38fee050557b)

![image](https://github.com/user-attachments/assets/f6ee399f-130c-414f-b283-77e52ffb0954)

```
cd shoeshop
```

## 4. Tiến hành copy dự án shoeshop vào thư mục chứa dựa án shoeshop ta vừa clone về

![image](https://github.com/user-attachments/assets/d8802e13-9394-4e8c-ade1-e3b0fa7a2349)

```
cd /projects/
ls
```

```
cp -rf shoeshop/* /root/data/shoeshop/
```

![image](https://github.com/user-attachments/assets/c7a90dba-112a-4f80-91ad-7584b0d8af05)

```
git status
```

-> checkout về nhánh main -> nếu đang ở nhánh main rồi thì thôi

```
git checkout -b main
```

```
git add .
git commit -m "feat(project): create base project"
git push -f origin main
```


```
git pull
```











