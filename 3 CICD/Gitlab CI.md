![image](https://github.com/user-attachments/assets/751a73e6-b38d-488a-852b-4fd5c450303f)

Gitlab CI/Continuous Deployment
--------

![image](https://github.com/user-attachments/assets/4fa2e128-967c-4b3c-a188-f2a3aca89df7)

Các bước thực hiện:

1/ Cài đặt công cụ tự động

2/ Viết file cấu hình công việc

-> Gitlab Runner

![image](https://github.com/user-attachments/assets/457fcc14-f9b1-4c05-a0ca-410ab1673f14)

## 1. Cài đặt công cụ Gitlab Runner

Link tham khảo:

```
https://elroydevops.tech/setup-gitlab-cicd-pipeline-private-registry/
```

```
apt-get update
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | bash
apt-get install gitlab-runner
apt-cache madison gitlab-runner
gitlab-runner -version
```

![image](https://github.com/user-attachments/assets/2021638b-c679-4447-926f-e272e7da79d7)

Tương tự như Nginx, các công cụ khác. Gitlab runner cũng sẽ có 1 user riêng.

```
cat /etc/passwd
```

![image](https://github.com/user-attachments/assets/d7b96d88-6062-435d-9a75-b6b4ff26b3ed)

Ta thấy 1 user là `gitlab runner`, thư mục làm việc sẽ là `/home/gitlab-runner`

## 2. Kết nối Gitlab runner với dự án

Quay lại dự án trên Gitlab

Settings -> Chọn CI/CD -> Chọn Runners -> Chọn Expand

![image](https://github.com/user-attachments/assets/83dcc503-ae8a-4f8d-8afc-0cf6922d32d2)

Bước tiếp theo về terminal

```
gitlab-runner register
```

![image](https://github.com/user-attachments/assets/b6fd7df1-a10d-4bed-b2bc-277cbbbe29e0)

Copy URL từ trình duyệt Gitlab và paste vào terminal

```
http://git.elroydevops.tech/
```

![image](https://github.com/user-attachments/assets/6d9e9caa-2db6-4c10-95f9-b70e1c14c07e)

Tiếp theo đến trường tocken

![image](https://github.com/user-attachments/assets/758033a0-8387-4fd2-959b-fa782d18cee6)

```
GR1348941fVgpVMX_Mbcqk4FusAdK
```

![image](https://github.com/user-attachments/assets/1ffc27a7-9805-4d86-a7b9-ca1a76894fcb)

![image](https://github.com/user-attachments/assets/909f31a3-e61c-4eec-abef-0aec2877713f)

Đặt cùng tên với gitlab-server

![image](https://github.com/user-attachments/assets/32f69fed-064c-49ce-95de-1196c47a90c9)

```
Enter tags for the runner (comma-separated):
```

Đây chính xác là tên con runner

Phần này Enter để bỏ qua

![image](https://github.com/user-attachments/assets/41416f49-05d7-4b49-9409-45aec18c2800)

ở đây ta sẽ chọn lựa chọn: `shell`

![image](https://github.com/user-attachments/assets/efee9ec3-f7a1-4675-8632-1700066298b2)

Như vậy ta đã có 1 file config được lưu trữ ở `etc/gitlab-runner/config.toml`

![image](https://github.com/user-attachments/assets/98bc0690-0392-44a2-8bee-90dbf61a4e0b)


Truy cập file config này:

```
nano /etc/gitlab-runner/config.toml
```

- concurrent: Nghĩa là con runner này có thể chạy đồng thời bao nhiêu dự án

![image](https://github.com/user-attachments/assets/adcf1732-0e80-48a0-b51c-345a5f3427fe)

Chạy câu lệnh để khởi động gitlab runner

```
 nohup gitlab-runner run --working-directory /home/gitlab-runner/ --config /etc/gitlab-runner/config.toml --service gitlab-runner --user gitlab-runner 2>&1 &
```

- nohup , 2>&1 & : cho phép chạy dưới nền
- --working-directory : chỉ định thư mục làm việc là `/home/gitlab-runner/`
- --config : lấy file config là `/etc/gitlab-runner/config.toml` để chạy
- --service : chỉ định dịch vụ này là dịch vụ nào `gitlab-runner`
- --user : chỉ định user để chạy là `gitlab-runner`

Kiểm tra github runner này đã chạy chưa

```
ps -ef| grep gitlab-runner
```

![image](https://github.com/user-attachments/assets/04724f8f-0c97-4645-9855-a6a2156c4c2e)

![image](https://github.com/user-attachments/assets/ff333066-1b22-4caf-bf8b-60902bfd09b4)

Như vậy ta thấy có 1 runner là gitlab-server đang chạy

![image](https://github.com/user-attachments/assets/d0f9e311-abf2-4c32-8dfd-c69cd7dab45d)

Tiếp theo bấm vào Edit

![image](https://github.com/user-attachments/assets/96bbd545-40de-4267-b5d1-12daa16f2853)

![image](https://github.com/user-attachments/assets/a563859c-0b8b-442f-91ce-940683cbb207)

- Active: Nếu như bỏ chọn thì runner này sẽ không được hoạt động
- Protected: Nếu như tick chọn vào thì chỉ những nhánh nào được bảo về thì mới được chạy thôi
- Run untagged jobs: Khi cấu hình kịch bản nó sẽ trực tiếp chạy trên những con runner được kết nối đến dự án đó mà k cần chỉ định chính xác 
- Lock to current projects: Khi tích chọn vào thì những dự án khác sẽ không sử dụng được runner này

![image](https://github.com/user-attachments/assets/1b931750-dbd4-497a-8deb-cbe5f16c1fe7)

Nhấn -> Save changes

## 3. Viết kịch bản 

Trước khi tạo file gitlab.ci

Settings -> Repository -> Protected branches -> Expand 

Đổi thêm quyền `push`

![image](https://github.com/user-attachments/assets/ce92e633-2632-4eeb-939e-5b0c36d936f0)

![image](https://github.com/user-attachments/assets/76538ed0-a273-41b1-a118-fb5b89ba3677)


New file

![image](https://github.com/user-attachments/assets/66f5c5a6-3bef-4fc0-b4ad-f1b96fecfb87)

![image](https://github.com/user-attachments/assets/8ac1bb58-78d7-4b12-972b-89a044c5634c)

```
stages:
    - build
    - deploy
    - checklog

build:
    stage: build
    script:
        - whoami
        - pwd
        - ls
    tags:
        - gitlab-server
```

```
Config(pipeline): add build stage
```

Chọn CI/CD để xem action đang chạy

![image](https://github.com/user-attachments/assets/7110e4d5-6eab-44aa-be5c-d16b3d6fb670)


```
stages:
    - build
    - deploy
    - checklog

build:
    stage: build
    before_script:
        - sudo apt-get update
        - sudo apt-get install -y maven
        - mvn -version 
    script:
        - mvn clean install -DskipTests
    tags:
        - gitlab-server
```

```
su gitlab-runner
cd /home/gitlab-runner/builds/4dzhtFov/0/shoeshop/shoeshop
```

![image](https://github.com/user-attachments/assets/a5ec54a8-962c-4601-8b01-7c005b20f740)

-> Như vậy ta đã thấy file `target` là build của dự án


Tiếp theo 

```
mkdir /datas
mkdir /datas/shoeshop
```

![image](https://github.com/user-attachments/assets/67d29577-1731-44f1-a76b-6fab3a9a2d21)

-> Cấu hình quan trọng: cho phép gitlab-runner sử dụng quyền sudo mà không cần password
------

```
visudo
```

![image](https://github.com/user-attachments/assets/5b34de83-e731-479a-8a9e-ebe12b7deed5)

```
gitlab-runner ALL=(ALL) NOPASSWD: /bin/cp*
gitlab-runner ALL=(ALL) NOPASSWD: /bin/choen*
gitlab-runner ALL=(ALL) NOPASSWD: /bin/su shoeshop*
```

![image](https://github.com/user-attachments/assets/92625409-47fc-462b-b937-09b44c71f85f)

hoặc

```
gitlab-runner ALL=(ALL) NOPASSWD: /bin/cp*, /bin/chown*, /bin/su shoeshop*
```

-> restart lại gitlab-runner

```
sudo systemctl restart gitlab-runner
```

gitlab-runner ALL=(ALL:ALL) được hiểu là áp dụng cho user,group ALL(user:group) -> để ALL=(ALL) là áp dụng cho tất cả

![image](https://github.com/user-attachments/assets/060c1923-1c31-4900-9c0c-29b52f983566)

Tạo thư mục mới

```
mkdir /datas
mkdir /datas/shoeshop
```

-> Cấu hình trên file .gitlab-ci.yml
-------------

![image](https://github.com/user-attachments/assets/e1143fde-817c-4c01-9be7-41a7dd17a19d)

```
stages:
  - build
  - deploy
  - checklog

build:
  stage: build
  variables:
        GIT_STRATEGY: clone 
  script:
    - echo "🔍 Kiểm tra JDK..."
    - |
      if which java > /dev/null && java -version 2>&1 | grep "17"; then
        echo "✅ JDK 17 đã được cài đặt."
      else
        echo "❌ JDK 17 chưa được cài."
      fi

    - echo "🔍 Kiểm tra Maven..."
    - |
      if which mvn > /dev/null; then
        echo "✅ Maven đã được cài đặt."
      else
        echo "❌ Maven chưa được cài."
      fi

    - echo "🔍 Kiểm tra MariaDB..."
    - |
      if systemctl is-active --quiet mariadb || which mariadb > /dev/null; then
        echo "✅ MariaDB đã được cài đặt và đang chạy."
      else
        echo "❌ MariaDB chưa được cài hoặc không hoạt động."
      fi

    - echo "🔍 Kiểm tra Docker..."
    - |
      if which docker > /dev/null && docker --version; then
        echo "✅ Docker đã được cài đặt."
      else
        echo "❌ Docker chưa được cài."
      fi

    - mvn install -DskipTests=true
  tags:
    - ubuntu-srv

deploy: 
  stage: deploy
  variables:
        GIT_STRATEGY: none
  script:
    - cd /home/gitlab-runner/builds/fyapp6-u/0/shoeshop/shoeshop/target
    - sudo cp shoe-ShoppingCart-0.0.1-SNAPSHOT.jar /datas/shoeshop/
    - cd /datas/shoeshop/
    - sudo chown -R shoeshop:shoeshop /datas/shoeshop
    - sudo su shoeshop
    - nohup java -jar shoe-ShoppingCart-0.0.1-SNAPSHOT.jar 2>&1 &
  tags:
    - ubuntu-srv

```

```
stages:
  - build
  - deploy
  - checklog

build:
  stage: build
  variables:
        GIT_STRATEGY: clone 
  script:
    - echo "🔍 Kiểm tra JDK..."
    - |
      if which java > /dev/null && java -version 2>&1 | grep "17"; then
        echo "✅ JDK 17 đã được cài đặt."
      else
        echo "❌ JDK 17 chưa được cài."
      fi

    - echo "🔍 Kiểm tra Maven..."
    - |
      if which mvn > /dev/null; then
        echo "✅ Maven đã được cài đặt."
      else
        echo "❌ Maven chưa được cài."
      fi

    - echo "🔍 Kiểm tra MariaDB..."
    - |
      if systemctl is-active --quiet mariadb || which mariadb > /dev/null; then
        echo "✅ MariaDB đã được cài đặt và đang chạy."
      else
        echo "❌ MariaDB chưa được cài hoặc không hoạt động."
      fi

    - echo "🔍 Kiểm tra Docker..."
    - |
      if which docker > /dev/null && docker --version; then
        echo "✅ Docker đã được cài đặt."
      else
        echo "❌ Docker chưa được cài."
      fi

    - mvn install -DskipTests=true
  tags:
    - ubuntu-srv

deploy:
  stage: deploy
  variables:
    GIT_STRATEGY: none
  script:
    # - echo "🔍 Kiểm tra và dừng tiến trình cũ trên cổng 8080 nếu có..."
    # - |
    #   PID=$(sudo lsof -ti:8080)
    #   if [ -n "$PID" ]; then
    #     echo "⚠️  Tiến trình cũ đang chạy với PID: $PID"
    #     echo "🛑 Dừng tiến trình cũ..."
    #     sudo kill -9 $PID
    #     sleep 5
    #   else
    #     echo "✅ Không có tiến trình nào chạy trên cổng 8080."
    #   fi

    - echo "🚀 Triển khai project mới..."
    - cd /home/gitlab-runner/builds/fyapp6-u/0/shoeshop/shoeshop/target
    - sudo cp shoe-ShoppingCart-0.0.1-SNAPSHOT.jar /datas/shoeshop/
    - sudo chown -R gitlab-runner:gitlab-runner /datas/shoeshop
    - sudo su shoeshop
    - cd /datas/shoeshop
    - ls
    - echo "🚀 Khởi chạy project mới..."
    - nohup java -jar /datas/shoeshop/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar --server.port=8081 > /datas/shoeshop/app.log 2>&1 &

    - echo "✅ Deployment hoàn tất!"
  tags:
    - ubuntu-srv
```

```
Config(pipeline): add script deploy stage
```

```
stages:
  - build
  - deploy
  - checklog

build:
  stage: build
  variables:
    GIT_STRATEGY: clone 
  script:
    - echo "🔍 Kiểm tra JDK..."
    - |
      if which java > /dev/null && java -version 2>&1 | grep "17"; then
        echo "✅ JDK 17 đã được cài đặt."
      else
        echo "❌ JDK 17 chưa được cài."
      fi

    - echo "🔍 Kiểm tra Maven..."
    - |
      if which mvn > /dev/null; then
        echo "✅ Maven đã được cài đặt."
      else
        echo "❌ Maven chưa được cài."
      fi

    - echo "🔍 Kiểm tra MariaDB..."
    - |
      if systemctl is-active --quiet mariadb || which mariadb > /dev/null; then
        echo "✅ MariaDB đã được cài đặt và đang chạy."
      else
        echo "❌ MariaDB chưa được cài hoặc không hoạt động."
      fi

    - echo "🔍 Kiểm tra Docker..."
    - |
      if which docker > /dev/null && docker --version; then
        echo "✅ Docker đã được cài đặt."
      else
        echo "❌ Docker chưa được cài."
      fi

    - mvn install -DskipTests=true
  tags:
    - ubuntu-srv

deploy:
  stage: deploy
  variables:
    GIT_STRATEGY: none
  script:
    - echo "🔍 Chuyển sang user 'shoeshop' để kiểm tra và kill tiến trình cũ..."
    - |
      sudo su shoeshop -c '
        OLD_PID=$(ps -ef | grep "shoe-ShoppingCart-0.0.1-SNAPSHOT.jar" | grep -v grep | awk "{print \$2}")
        if [ -n "$OLD_PID" ]; then
          echo "⚠️ Tiến trình cũ đang chạy với PID: $OLD_PID"
          echo "🛑 Dừng tiến trình cũ..."
          kill -9 $OLD_PID || echo "⚠️ Không thể kill PID $OLD_PID"
          sleep 5
        else
          echo "✅ Không có tiến trình shoeshop cũ đang chạy."
        fi
      '

    - echo "🚀 Copy project mới..."
    - cd /home/gitlab-runner/builds/fyapp6-u/0/shoeshop/shoeshop/target
    - sudo cp shoe-ShoppingCart-0.0.1-SNAPSHOT.jar /datas/shoeshop/
    - sudo chown -R gitlab-runner:gitlab-runner /datas/shoeshop

    - echo "🔑 Chuyển sang user 'shoeshop' để triển khai..."
    - sudo su shoeshop
    - cd /datas/shoeshop
    - ls
    - echo "🚀 Khởi chạy project mới..."
    - nohup java -jar /datas/shoeshop/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar --server.port=8081 > /datas/shoeshop/app.log 2>&1 &

    - echo "✅ Deployment hoàn tất!"
  tags:
    - ubuntu-srv

```

```
kill -9 $( ps -ef| grep shoe-ShoppingCart-0.0.1-SNAPSHOT.jar | grep -v grep | awk '{print $2}')
```

```
ps -ef| grep shoeshop
````

```
sudo ufw allow 8081
sudo ufw reload
```

![image](https://github.com/user-attachments/assets/6a6f29f0-ce36-49f4-899b-6150d38384cb)


Đặt biến sử dụng
------
```
variables:
  projectname: shoe-ShoppingCart
  version: 0.0.1-SNAPSHOT
  projectuser: shoeshop
  projectpath: /datas/$projectuser
  port: 8081


stages:
  - build
  - deploy
  - checklog

build:
  stage: build
  variables:
    GIT_STRATEGY: clone 
  script:
    - echo "🔍 Kiểm tra JDK..."
    - |
      if which java > /dev/null && java -version 2>&1 | grep "17"; then
        echo "✅ JDK 17 đã được cài đặt."
      else
        echo "❌ JDK 17 chưa được cài."
      fi

    - echo "🔍 Kiểm tra Maven..."
    - |
      if which mvn > /dev/null; then
        echo "✅ Maven đã được cài đặt."
      else
        echo "❌ Maven chưa được cài."
      fi

    - echo "🔍 Kiểm tra MariaDB..."
    - |
      if systemctl is-active --quiet mariadb || which mariadb > /dev/null; then
        echo "✅ MariaDB đã được cài đặt và đang chạy."
      else
        echo "❌ MariaDB chưa được cài hoặc không hoạt động."
      fi

    - echo "🔍 Kiểm tra Docker..."
    - |
      if which docker > /dev/null && docker --version; then
        echo "✅ Docker đã được cài đặt."
      else
        echo "❌ Docker chưa được cài."
      fi

    - mvn install -DskipTests=true
  tags:
    - ubuntu-srv

deploy:
  stage: deploy
  variables:
    GIT_STRATEGY: none
  script:
    - echo "🔍 Chuyển sang user '$projectuser' để kiểm tra và kill tiến trình cũ..."
    - |
      sudo su $projectuser -c '
        OLD_PID=$(ps -ef | grep "$projectname-$version.jar" | grep -v grep | awk "{print \$2}")
        if [ -n "$OLD_PID" ]; then
          echo "⚠️ Tiến trình cũ đang chạy với PID: $OLD_PID"
          echo "🛑 Dừng tiến trình cũ..."
          kill -9 $OLD_PID || echo "⚠️ Không thể kill PID $OLD_PID"
          sleep 5
        else
          echo "✅ Không có tiến trình $projectuser cũ đang chạy."
        fi
      '

    - echo "🚀 Copy project mới..."
    - cd /home/gitlab-runner/builds/fyapp6-u/0/$projectuser/$projectuser/target
    - sudo cp $projectname-$version.jar /datas/$projectuser/
    - sudo chown -R gitlab-runner:gitlab-runner $projectpath

    - echo "🔑 Chuyển sang user '$projectuser' để triển khai..."
    - sudo su $projectuser
    - cd $projectpath
    - ls
    - echo "🚀 Khởi chạy project mới..."
    - nohup java -jar /datas/$projectuser/$projectname-$version.jar --server.port=$port > /datas/$projectuser/app.log 2>&1 &

    - echo "✅ Deployment hoàn tất!"
  tags:
    - ubuntu-srv

```

```
ps -ef| grep shoeshop
```

![image](https://github.com/user-attachments/assets/07b447b8-3f4b-4901-9f30-3c4d406c850a)








