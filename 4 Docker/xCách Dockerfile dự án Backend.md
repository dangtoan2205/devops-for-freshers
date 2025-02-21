Cách Dockerfile dự án Backend
----------------

## 1. Tạo file Docker file

```
mkdir data
mkdir data/shoeshop
```

![image](https://github.com/user-attachments/assets/9979a83f-8b39-4f16-b9b8-3dcbfec7539e)

```
cd /home/ubuntu/
cp -r shoeshop/. /home/data/shoeshop/
```
![image](https://github.com/user-attachments/assets/2b497912-0a75-45f8-b8b2-0eeeb68db8f5)

```
nano Dockerfile
```

```
## build stage ##

# Sử dụng Maven với JDK 17 trên Alpine
FROM maven:3.8.7-jdk-17-alpine as build

# Thiết lập thư mục làm việc
WORKDIR /app

# Sao chép mã nguồn vào container
COPY . .

# Biên dịch ứng dụng
RUN mvn install -DskipTests=true

## run stage ##

# Chạy ứng dụng (giả sử tệp JAR nằm trong target)
FROM amazoncorretto:8u442-alpine3.21-jre

WORKDIR /run

COPY --from=build /app/target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar

EXPOSE 8080

ENTRYPOINT java -jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar
```

```
## build stage ##
FROM maven:3.5.3-jdk-8-alpine AS build

WORKDIR /app

COPY . .

RUN mvn install -DskipTests=true

## run stage ##
FROM amazoncorretto:8u402-alpine-jre

WORKDIR /run

COPY --from=build /app/target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar

EXPOSE 8080

ENTRYPOINT java -jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar
```

**Build**

```
## build stage ##
FROM maven:3.5.3-jdk-8-alpine AS build

WORKDIR /app

COPY . .

RUN mvn install -DskipTests=true

## run stage ##
FROM amazoncorretto:8u402-alpine-jre

WORKDIR /run

COPY --from=build /app/target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar"]
```

Xóa cache Docker và build lại:

```
docker system prune -a
```

```
docker build -t shoeshop:v1 .
```

```
docker images
```

![image](https://github.com/user-attachments/assets/019564b8-1270-4f6e-be9f-b2ef6e465c87)

**RUN**

```
docker run --name shoeshop -dp 8888:8080 shoeshop:v1
```








































