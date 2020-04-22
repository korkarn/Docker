# Docker : Create container > MySQL & phpMyAdmin
## Environment
- OS : Windows 10 Pro
- Docker : Ver. 19.03 https://docs.docker.com/docker-for-mac/install/

Open a terminal and run the command as administrator below in order to check your docker installation

Ref: https://hub.docker.com/_/rabbitmq
```
$ docker pull rabbitmq
```
```
$ docker run -d --hostname my-rabbit --name some-rabbit rabbitmq:3-management
```
```
$ docker ps -a
```
```
$ docker run -p 5671-5672:5671-5672 -p 4369:4369 -p 15671-15672:15671-15672 -p 25672:25672 -td rabbitmq:3-management
```
```
$ docker ps -a
```