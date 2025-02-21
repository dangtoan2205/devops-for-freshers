Triển khai dự án CI/CD theo cách thủ công
----------

Để thực hiện bước CI/CD thủ công thì ta thêm như sau:

```
when: manual
```

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
  - showlog

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
  only:
    - tags

deploy:
  stage: deploy
  variables:
    GIT_STRATEGY: none
  when: manual
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
    - sudo cp $projectname-$version.jar $projectpath
    - sudo chown -R gitlab-runner:gitlab-runner $projectpath

    - echo "🔑 Chuyển sang user '$projectuser' để triển khai..."
    - sudo su $projectuser
    - cd $projectpath
    - ls
    - echo "🚀 Khởi chạy project mới..."
    - nohup java -jar $projectpath/$projectname-$version.jar --server.port=$port > $projectpath/nohup.out 2>&1 &

    - echo "✅ Deployment hoàn tất!"
  tags:
    - ubuntu-srv
  only:
    - tags

showlog:
  stage: showlog
  variables:
    GIT_STRATEGY: none
  when: manual
  script:
    - echo "🔍 Kiểm tra log dự án..."
    - sudo su $projectuser
    - cd $projectpath
    - ls
    - tail -n 10000 nohup.out
  tags:
    - ubuntu-srv
  only:
    - tags
```

```
Config(pipline): change the project deployment strategy from CDeployment CDelivery
```

-> Tạo tags

`dev_0.0.3`

![image](https://github.com/user-attachments/assets/3593de99-5cf1-4fb9-b7b2-d357843cad2d)

-> Ta sẽ phải bấm chạy thủ công bằng tay

-> Mục đích chạy tự động

- Chỉ có người nào có đủ quyền để xác nhận (VD: Cần triển khai lên môi trường cao hơn thì code đó sẽ phải quét qua những công cụ bảo mật hoặc là clean code và phải cần có một người approved thì mới triển khai lên các môi trường cao hơn.)

Cần thêm điều kiện để user đó có quyền approved

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
  - showlog

build:
  stage: build
  variables:
    GIT_STRATEGY: clone 
  script:
    # - echo "🔍 Kiểm tra JDK..."
    # - |
    #   if which java > /dev/null && java -version 2>&1 | grep "17"; then
    #     echo "✅ JDK 17 đã được cài đặt."
    #   else
    #     echo "❌ JDK 17 chưa được cài."
    #   fi

    # - echo "🔍 Kiểm tra Maven..."
    # - |
    #   if which mvn > /dev/null; then
    #     echo "✅ Maven đã được cài đặt."
    #   else
    #     echo "❌ Maven chưa được cài."
    #   fi

    # - echo "🔍 Kiểm tra MariaDB..."
    # - |
    #   if systemctl is-active --quiet mariadb || which mariadb > /dev/null; then
    #     echo "✅ MariaDB đã được cài đặt và đang chạy."
    #   else
    #     echo "❌ MariaDB chưa được cài hoặc không hoạt động."
    #   fi

    # - echo "🔍 Kiểm tra Docker..."
    # - |
    #   if which docker > /dev/null && docker --version; then
    #     echo "✅ Docker đã được cài đặt."
    #   else
    #     echo "❌ Docker chưa được cài."
    #   fi

    - mvn install -DskipTests=true
  tags:
    - ubuntu-srv
  only:
    - tags

deploy:
  stage: deploy
  variables:
    GIT_STRATEGY: none
  when: manual
  script:
    - >
      if echo "toandn" | grep -wq "$GITLAB_USER_LOGIN"; then
        echo "✅ Người dùng hợp lệ: $GITLAB_USER_LOGIN"
        echo "🔍 Chuyển sang user '$projectuser' để kiểm tra và kill tiến trình cũ..."
        sudo su $projectuser -c '
          OLD_PID=$(ps -ef | grep "$projectname-$version.jar" | grep -v grep | awk "{print \$2}")
          if [ -n "$OLD_PID" ]; then
            echo "⚠️ Tiến trình cũ đang chạy với PID: $OLD_PID"
            echo "🛑 Dừng tiến trình cũ..."
            kill -9 $OLD_PID || echo "⚠️ Không thể kill PID $OLD_PID"
            sleep 5
          else
            echo "✅ Không có tiến trình $projectname cũ đang chạy."
          fi
        '

        echo "🚀 Copy project mới..."
        cd /home/gitlab-runner/builds/fyapp6-u/0/$projectuser/$projectuser/target
        sudo cp $projectname-$version.jar $projectpath
        sudo chown -R gitlab-runner:gitlab-runner $projectpath

        echo "🔑 Chuyển sang user '$projectuser' để triển khai..."
        sudo su $projectuser
        cd $projectpath
        ls
        echo "🚀 Khởi chạy project mới..."
        nohup java -jar $projectpath/$projectname-$version.jar --server.port=$port > $projectpath/nohup.out 2>&1 &

        echo "✅ Deployment hoàn tất!"
      else
        echo "❌ Permission denied. User '$GITLAB_USER_LOGIN' không có quyền deploy."
        exit 1
      fi
  tags:
    - ubuntu-srv
  only:
    - tags

showlog:
  stage: showlog
  variables:
    GIT_STRATEGY: none
  when: manual
  script:
    - echo "🔍 Kiểm tra log dự án..."
    - sudo su $projectuser
    - cd $projectpath
    - ls
    - tail -n 10000 nohup.out
  tags:
    - ubuntu-srv
  only:
    - tags

```

```
config(pipline): change permission
```


Thêm secret gitlab để bảo mật thông tin user có quyền deploy ci/cd
-------

- Vào `Project Settings` > `CI/CD` > `Variables`.
- Nhấn `Add variable` và nhập:
  - Key: USER_ADMIN
  - Value: toandn
- Type: `Masked` (Ẩn trong log) và `Protected` (Chỉ dùng cho protected branch/tags).
- Lưu thay đổi.


- Cập nhật `.gitlab-ci.yml` để sử dụng **Secret Variables**

```
- >
  if [ "$GITLAB_USER_LOGIN" == "$USER_ADMINS" ]; then
    echo "✅ Người dùng hợp lệ: $GITLAB_USER_LOGIN"
```

```
$USER_ADMINS
```

![image](https://github.com/user-attachments/assets/10a3afae-b714-4903-8ff5-41251f367b28)


Code

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
  - showlog

build:
  stage: build
  variables:
    GIT_STRATEGY: clone 
  script:
    # - echo "🔍 Kiểm tra JDK..."
    # - |
    #   if which java > /dev/null && java -version 2>&1 | grep "17"; then
    #     echo "✅ JDK 17 đã được cài đặt."
    #   else
    #     echo "❌ JDK 17 chưa được cài."
    #   fi

    # - echo "🔍 Kiểm tra Maven..."
    # - |
    #   if which mvn > /dev/null; then
    #     echo "✅ Maven đã được cài đặt."
    #   else
    #     echo "❌ Maven chưa được cài."
    #   fi

    # - echo "🔍 Kiểm tra MariaDB..."
    # - |
    #   if systemctl is-active --quiet mariadb || which mariadb > /dev/null; then
    #     echo "✅ MariaDB đã được cài đặt và đang chạy."
    #   else
    #     echo "❌ MariaDB chưa được cài hoặc không hoạt động."
    #   fi

    # - echo "🔍 Kiểm tra Docker..."
    # - |
    #   if which docker > /dev/null && docker --version; then
    #     echo "✅ Docker đã được cài đặt."
    #   else
    #     echo "❌ Docker chưa được cài."
    #   fi

    - mvn install -DskipTests=true
  tags:
    - ubuntu-srv
  only:
    - tags

deploy:
  stage: deploy
  variables:
    GIT_STRATEGY: none
  when: manual
  script:
    - >
      if echo "$USER_ADMINS" | grep -wq "$GITLAB_USER_LOGIN"; then
        echo "✅ Người dùng hợp lệ: $GITLAB_USER_LOGIN"
        echo "🔍 Chuyển sang user '$projectuser' để kiểm tra và kill tiến trình cũ..."
        sudo su $projectuser -c '
          OLD_PID=$(ps -ef | grep "$projectname-$version.jar" | grep -v grep | awk "{print \$2}")
          if [ -n "$OLD_PID" ]; then
            echo "⚠️ Tiến trình cũ đang chạy với PID: $OLD_PID"
            echo "🛑 Dừng tiến trình cũ..."
            kill -9 $OLD_PID || echo "⚠️ Không thể kill PID $OLD_PID"
            sleep 5
          else
            echo "✅ Không có tiến trình $projectname cũ đang chạy."
          fi
        '

        echo "🚀 Copy project mới..."
        cd /home/gitlab-runner/builds/fyapp6-u/0/$projectuser/$projectuser/target
        sudo cp $projectname-$version.jar $projectpath
        sudo chown -R gitlab-runner:gitlab-runner $projectpath

        echo "🔑 Chuyển sang user '$projectuser' để triển khai..."
        sudo su $projectuser
        cd $projectpath
        ls
        echo "🚀 Khởi chạy project mới..."
        nohup java -jar $projectpath/$projectname-$version.jar --server.port=$port > $projectpath/nohup.out 2>&1 &

        echo "✅ Deployment hoàn tất!"
      else
        echo "❌ Permission denied. User '$GITLAB_USER_LOGIN' không có quyền deploy."
        exit 1
      fi
  tags:
    - ubuntu-srv
  only:
    - tags

showlog:
  stage: showlog
  variables:
    GIT_STRATEGY: none
  when: manual
  script:
    - echo "🔍 Kiểm tra log dự án..."
    - sudo su $projectuser
    - cd $projectpath
    - ls
    - tail -n 10000 nohup.out
  tags:
    - ubuntu-srv
  only:
    - tags

```


