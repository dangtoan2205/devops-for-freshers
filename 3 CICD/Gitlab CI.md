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









































