 A Sample spring boot Application 

  - apt install default-jdk
  - apt install maven
  - git clone https://github.com/gopal1409/spring-hello.git
  - cd spring-hello/
  - cd target/
  - ls
  - java -jar spring-boot-2-hello-world-1.0.2-SNAPSHOT.jar

open DNS url with port :http://lab-as-19148.lc.cl-labs.springpeople.com:8080/hello


### Custom Image with docker file 

#### create a dockerfile without extension 
#### inside the dockerfile provide this instruction

FROM openjdk
<br>
#when i run this will download the openjdk image<br>
WORKDIR /app<br>
#this will create the directory inside your image if the directory exist it is going to skip creation of the directory<br>
COPY /target/*.jar /app/web.jar<br>
#finally we are going to run the application<br>
ENTRYPOINT ["java","-jar","/app/web.jar"]<br>

### build docker image
docker build -t app . <br>

### run the image on the container
docker container run -d -p 81:8080 app<br>

### check in curl
curl localhost:81/hello<br>

<img width="628" height="121" alt="image" src="https://github.com/user-attachments/assets/3fcd0bfe-5662-4e11-a9d4-e2b35af5bfa0" />

### DOCKER HUB

 - docker login
- docker images
- docker tag 767f5447a050 mysticmac7/training
- docker push mysticmac7/training

<img width="899" height="545" alt="image" src="https://github.com/user-attachments/assets/089453d8-e9db-4d82-96f7-436227aecf5c" />
