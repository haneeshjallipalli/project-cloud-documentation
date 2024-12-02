
# Run your Spring Boot application in Docker containers

## Switch to superuser:
```sh
sudo su -
 ```
 ## Install Docker:
 ```sh
 yum install -y docker
 ```
 ## Start Docker service:
```sh
 systemctl start docker
 ```

## Enable Docker service on boot:
```sh
systemctl enable docker
 ```

##  Download Docker Compose:
```sh
curl -SL https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
 ```

## Set executable permissions for Docker Compose:
```sh
chmod +x /usr/local/bin/docker-compose
 ```
## Create a symbolic link for Docker Compose:
```sh
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
 ```
## Update system packages:
```sh
yum update -y
 ```

## Install Maven:
```sh
yum install -y maven
 ```
## Now update maven to 3.2.5:
```sh
wget http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.2.5/binaries/apache-maven-3.2.5-bin.tar.gz
tar xzf apache-maven-3.2.5-bin.tar.gz
sudo mv apache-maven-3.2.5 /opt/
 ```
## Set Maven environment variables:
```sh
export PATH=/opt/apache-maven-3.2.5/bin:$PATH
source ~/.bashrc
 ```
## Check Maven version:
```sh
mvn -version
 ```
## Install Git:
```sh
yum install -y git
 ```
## Clone a Git repository:
```sh
git clone https://github.com/HaneeshDevops/SpringBootEcommerceApplication.git
 ```
## Navigate to the project directory:
```sh
cd SpringBootEcommerceApplication
 ```
## Clean Maven project:
```sh
mvn clean
 ```
## Build Maven project:
```sh
mvn install -DskipTests
 ```
## Create a Dockerfile::
```sh
vi Dockerfile
 ```
## Paste the following in Dockerfile
```sh
FROM openjdk:17
ARG CONTAINER_NAME=ecomapp
ARG IMAGE_NAME=ecomapp
EXPOSE 9090
ADD target/EcommereceApp-rest-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
 ```
## Create a docker-compose.yml file:
```sh
vi docker-compose.yml
 ```
 ## Paste the following in docker-compose.yml
 ```sh
 version: "3.8"
services:
 postgres:
   image: postgres:latest
   container_name: postgres_db
   restart: always
   environment:
     POSTGRES_USER: postgres
     POSTGRES_PASSWORD: root
     POSTGRES_DB: TEST2
   volumes:
     - postgres-data:/var/lib/postgresql/data
 app:
   build: .
   container_name: myapplication
   depends_on:
     - postgres
   environment:
     SPRING_DATASOURCE_URL: jdbc:postgresql://postgres:5432/TEST2
     SPRING_DATASOURCE_USERNAME: postgres
     SPRING_DATASOURCE_PASSWORD: root
   ports:
     - "9090:9090"
volumes:
 postgres-data:
 ```
## Build image and start Docker containers::
```sh
docker-compose up -d
 ```
 ## Check images:
```sh
docker images
 ```
## Check running containers:
```sh
docker ps
 ```
## Check application logs:
```sh
docker logs myapplication
 ```
## Inspect a Docker container:
```sh
docker inspect postgres_db
 ```
# ALL COMMANDS AT ONCE:
```sh
sudo su -
yum install -y docker
systemctl start docker
systemctl enable docker
curl -SL https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
sudo yum update -y
sudo yum install -y maven
wget http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.2.5/binaries/apache-maven-3.2.5-bin.tar.gz
tar xzf apache-maven-3.2.5-bin.tar.gz
sudo mv apache-maven-3.2.5 /opt/
export PATH=/opt/apache-maven-3.2.5/bin:$PATH
source ~/.bashrc
mvn -version
sudo yum install -y git
git clone https://github.com/HaneeshDevops/SpringBootEcommerceApplication.git
cd SpringBootEcommerceApplication
mvn clean
mvn install -DskipTests
docker-compose up -d
docker images
docker ps
docker logs myapplication
docker ps
```

## Swagger UI Link
```
http://localhost:8084/swagger-ui/index.html
```
