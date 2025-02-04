Câu lệnh trong Linux
---------


## Đổi tên của Server chính là HostName
![image](https://github.com/user-attachments/assets/4c4cacbe-d2a2-4a65-92c4-aa911f650722)

```
sudo hostnamectl set-hostname lab-server

sudo reboot
```

## Kiểm tra các cổng kết nối
![image](https://github.com/user-attachments/assets/c73ad875-e331-4beb-8e90-435adeed93ea)

```
sudo netstat -tlpun
```

Giải thích:
  - t: hiển thị thông tin về các kết nối TCP
  - l: hiển thị các cổng đang mở và lắng nghe để chấp nhận kết nối
  - p: hiển thị tiến trình và chương trình liên quan đến mỗi kết nối
  - u: hiển thị thông tin về các kết nối UDP
  - n: hiển thị địa chỉ IP và cổng

## Kiểm tra các tiến trình đang chạy trên hệ thống
```
sudo ps -ef
```

##  Kiểm tra Server này có thông kết nối đến Server khác với port cụ thể hay không
![image](https://github.com/user-attachments/assets/7a5473b5-6786-472c-a4bd-8babeab10b04)

```
sudo telnet 192.168.1.199 80
```
VD: 
192.168.1.199 - là địa chỉ IP của Database 

80 - đây là port

## Lệnh để kiểm tra đường truyền từ nguồn tới đích có bị chặn ở đâu không (cụ thể đường đi)
![image](https://github.com/user-attachments/assets/e611bf6b-5a7d-404a-a048-bd4b2565bff0)

```
traceroute -T -p 80 192.168.1.199
```

Giải thích:
  - T : Giao thức kết nối TCP
  - p : Port kết nối

## Xóa công cụ bất kì mà mình đã cài đặt
![image](https://github.com/user-attachments/assets/9b3fd237-9ff2-4bcc-a57d-536b981a97d9)

```
sudo apt remove net-tools
```




























