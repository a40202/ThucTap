# Bước 1: Build ứng dụng bằng Maven
# Sử dụng image maven chính thức để đóng gói file JAR
FROM maven:3.8.5-openjdk-17 AS build
WORKDIR /app

# Copy file pom.xml và tải các dependencies trước để tận dụng cache của Docker
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy toàn bộ mã nguồn và thực hiện build
COPY src ./src
RUN mvn clean package -DskipTests

# Bước 2: Chạy ứng dụng bằng JDK nhẹ (Slim)
# Giúp giảm dung lượng image và tăng tốc độ khởi động trên Railway/Render
FROM openjdk:17-jdk-slim
WORKDIR /app

# Copy file JAR từ bước build sang
# Tên file JAR build ra thường là khoacntt-0.0.1-SNAPSHOT.jar
COPY --from=build /app/target/*.jar app.jar

# Khai báo cổng ứng dụng (Spring Boot mặc định là 8080)
EXPOSE 8080

# Chạy ứng dụng và quan trọng nhất là gán cổng theo biến môi trường ${PORT} của server
ENTRYPOINT ["java", "-jar", "app.jar", "--server.port=${PORT:8080}"]