![image](https://github.com/user-attachments/assets/8e710a71-0be4-4d8d-9ac5-14f30cf05af1)

![image](https://github.com/user-attachments/assets/fadacddc-a434-4ea3-b32d-119b98853768)


Câu lệnh sử dụng trong Docker
---

- List container
```
docker ps -a
```

- Stop container
```
docker stop ubuntu
```

- Delete container
```
docker rm car-serv
```

```
docker rm -f car-serv
```

------

- List image
```
docker images
```

- Delete image
```
docker rmi ubuntu:22.04
```

![image](https://github.com/user-attachments/assets/7dc55f53-e764-4639-8ca6-2df82c940f49)


- Xóa bỏ container hiện có trên server
```
docker rm -f $(docker ps -a)
```



















