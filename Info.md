<img width="1277" height="737" alt="image" src="https://github.com/user-attachments/assets/41cb31bf-678e-425b-af1d-e12e83805eac" />
<br>
<img width="1291" height="727" alt="image" src="https://github.com/user-attachments/assets/9c4b082c-ea68-460e-ada7-4a9af53461f2" />
<br>
<img width="1315" height="764" alt="image" src="https://github.com/user-attachments/assets/cc66c5cf-eaf0-418f-b32e-ee9c50205292" />

<br>
<br>
<br>
Install Docker : https://docs.docker.com/engine/install/ubuntu/
- detach is use to run the command on forground and not interupt the new command to enter
test to check nginx  : docker container run --publish 80:80 --detach nginx  
-> this will check for image from docker image and download only once if not found  : https://hub.docker.com/

- to check docker running process : docker ps
- lsblk (HD space in ubuntu)

This will show the docker server information : docker info

###docker ps -a will show all the running and stopped container
 - docker ps -a ( all process running)

 <img width="1406" height="117" alt="image" src="https://github.com/user-attachments/assets/fc0af2b9-175c-44bc-8ed6-29b2c4720ed0" />

 ### Second way with run
docker run --name mongo -d mongo

###top is a command in linux which is used to check all your process id 
[root@ip-172-31-115-189 ~]$ docker top mongo
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
lxd                 8088                8065                2                   08:45               ?                   00:00:00            mongod --bind_ip_all

<img width="1232" height="126" alt="image" src="https://github.com/user-attachments/assets/3d96bcf3-048f-4db1-8bc6-946808b25531" />

#####docker networking 

<img width="451" height="94" alt="image" src="https://github.com/user-attachments/assets/2f601d4b-91ed-411f-b35f-59e04cbb4a58" />

#this will create a custom network 
  109  docker network create my_app_net
  110  docker network ls
#we are creating two different container added to your custom network
  111  docker run -d --name new_nginx --network my_app_net nginx
  112  docker run -d --name my_nginx --network my_app_net nginx
  113  docker network ls
##if you check your custom network you will see there are two container on the custom network
  114  docker network inspect my_app_net

<img width="791" height="873" alt="image" src="https://github.com/user-attachments/assets/b27bc7ef-e5fc-463d-86d1-5b35df9ef67e" />


### Persistent volume

 - docker volume ls
 <img width="396" height="60" alt="image" src="https://github.com/user-attachments/assets/1b526d24-c79b-4ec3-a3b9-d11ff49a9b5f" />

 labeled volume
 - docker run -d --name db1 -e MYSQL_ALLOW_EMPTY_PASSWORD=true -v mysql-db:/var/lib/mysql mysql:8
 <img width="892" height="313" alt="image" src="https://github.com/user-attachments/assets/a139e57d-23de-4a30-b94c-ed7972b9c8e7" />

<img width="1203" height="497" alt="image" src="https://github.com/user-attachments/assets/551dbf9e-9f08-48c4-8b19-6841188d9bee" />

#second exercise we delete the database then also the data persist
<img width="1663" height="655" alt="image" src="https://github.com/user-attachments/assets/14f37ed7-bc2a-4399-b08c-a4f0c9b16d6c" />

- stop and remove container
 <img width="567" height="157" alt="image" src="https://github.com/user-attachments/assets/e1285024-3ef2-4fdf-ba4b-311e9243c024" />

 - add the pesisten path to new DB:

   <img width="908" height="536" alt="image" src="https://github.com/user-attachments/assets/e8380bdb-1e59-45f7-87ff-1c91772f7911" />

- mapping to new db with persistent data:
<img width="586" height="76" alt="image" src="https://github.com/user-attachments/assets/05909f21-7240-4567-9c59-94d7b1dd78e7" />


