# Docker : Create container > MySQL & phpMyAdmin
## Environment
- OS : Windows 10 Pro
- Docker : Ver. 19.03 https://docs.docker.com/docker-for-mac/install/

## Step 1 : Obtaining and running MySQL docker container
Open a terminal and run the command as administrator below in order to check your docker installation
```
$ docker version
```
```
Client: Docker Engine - Community
 Version:           19.03.8
 API version:       1.40
 Go version:        go1.12.17
 Git commit:        afacb8b
 Built:             Wed Mar 11 01:23:10 2020
 OS/Arch:           windows/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.8
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.17
  Git commit:       afacb8b
  Built:            Wed Mar 11 01:29:16 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683

```

https://hub.docker.com/_/mysql
```
$ docker pull mysql
```
MySQL version 8.0.1
```
$ docker run --name [NAME] -p 3306:3306 -e MYSQL_ROOT_PASSWORD=[PASSWORD] -d mysql
```
- The option --name allows us to assign a specific name for our running container.
- The option -p is set external:internal container port
- The option -e is used to pass a value for the container environment variable MYSQL_ROOT_PASSWORD. This variable is requested by the image to
run properly and it will be assigned to the root password of MySQL. More information about this image can be found in docker hub (here).
- The option -d means that docker will run the container in the background in “detached” mode. If -d is not used the container run in the default foreground mode.

check container is running
```
$ docker ps -a
```
```
CONTAINER ID     IMAGE     NAMES            COMMAND                  CREATED             STATUS              PORTS
8df40720f193     mysql     my-own-mysql     "docker-entrypoint.s…"   3 seconds ago       Up 2 seconds        0.0.0.0:3306->3306/tcp, 33060/tcp 
```
If you didn't set password
```
$ docker run --name [NAME] -p 3306:3306 -e MYSQL_ROOT_PASSWORD= -d mysql
```
Container will be error
```
CONTAINER ID     IMAGE     NAMES            COMMAND                  CREATED             STATUS                        PORTS
8df40720f193     mysql     my-own-mysql     "docker-entrypoint.s…"   10 seconds ago      Exited (1) 9 seconds ago 
```

## Step 2: Obtaining and running phpMyAdmin docker container
https://hub.docker.com/r/phpmyadmin/phpmyadmin/
```
$ docker pull phpmyadmin/phpmyadmin:latest
```
```
$ docker run --name [name] -d --link [mySQL name or Container ID of mySQL]:db -p 8081:80 phpmyadmin/phpmyadmin
```
- The options name and d has been explained in the previous section.
- The option --link provides access to another container running in the host. In our case the container is the one created in the previous section, called my-own-mysql and the resource accessed is the MySQL db.
- The mapping between the host ports and the container ports is done using the option -p followed by the port number of the host (8081) that will be redirected to the port number of the container (80, where the ngix server with the phpMyAdmin web app is installed).

check container is running
```
$ docker ps -a
```
```
CONTAINER ID     IMAGE                   NAMES              COMMAND                  CREATED             STATUS              PORTS
eab072242dbc     phpmyadmin/phpmyadmin   my-own-phpmyadmin  "/docker-entrypoint.…"   5 hours ago         Up 5 hours          0.0.0.0:8081->80/tcp 
```

## Step 3: Access phpMyAdmin
 http://localhost:8081

 username : root

## Step 4: Import sql file
1. create DB
2. import button -> Go

## Step 5: Set Authenticate for mySQL v.8

NodeJS connect to mySQL Problem  
```
ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL client
```
Open a terminal and run the command as administrator
```
$ docker exec -it [Container ID] bash
```
```
root@[Container ID]:/# _
```
```
$ mysql -u root -p
```
Enter passsword
```
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 503
Server version: 8.0.19 MySQL Community Server - GPL

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```
```
$ CREATE USER '[username]'@'%' IDENTIFIED WITH mysql_native_password BY '[password]';
```
```
Query OK, 0 rows affected (0.02 sec)
```
```
$ GRANT ALL ON [DB name].* TO '[username]'@'%' WITH GRANT OPTION;
```
```
Query OK, 0 rows affected (0.02 sec)
```
Access phpMyAdmin with new username and password
Try to connect mySQL with NodeJS or other language