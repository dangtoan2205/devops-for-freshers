CI/CD là gì? CI/CD để làm gi?
----------

![image](https://github.com/user-attachments/assets/7794aef5-3371-4b48-8216-53fc6099405a)

-> Dev sẽ code các dựa án -> Rồi sau đó Ops hay System sẽ tiền hành lấy code và triển khai dự án

![image](https://github.com/user-attachments/assets/116d3f0e-3d08-497b-884c-9c953ed25646)

Quá trình phát triển dự án gooomfm 3 môi trường chính:

- development : dành cho dev
- staging : dành cho test
- production : dành cho end-user

![image](https://github.com/user-attachments/assets/f8855d6f-ab70-4766-8e56-a6437b33808f)

Quá trình CI/CD

CI: 
- Clone code, build dự án rồi tích hợp các công cụ test dự án như: perfomance, clean code, security, ...

CD: 
- Continuous deployment: Triển khai hoàn toàn tự động. (Chỉ cần 1 dòng commit code là chức năng mới sẽ được triển khai mà không cần bước nào cả)
- Continuous delivery: Triển khai thủ công. (Chúng ta sẽ cần 1 bước xác nhận để triển khai dự án)

----------------

![image](https://github.com/user-attachments/assets/ace3ddfd-b099-4abb-80ba-9abf92374c73)

Có 2 công cụ chính dùng để CI/CD:
- Gitlab CI/CD
- Jenkins









































