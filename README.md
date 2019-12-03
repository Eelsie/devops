# DevOps with Docker

### 1.1
```
$ docker ps -a
CONTAINER ID        IMAGE                               COMMAND                   CREATED              STATUS                          PORTS                    NAMES
75cd644b4958        nginx                               "nginx -g 'daemon of…"    11 seconds ago       Up 9 seconds                    80/tcp                   affectionate_borg
0bdcbd877b1c        nginx                               "nginx -g 'daemon of…"    About a minute ago   Exited (0) About a minute ago                            fervent_mendeleev
c080d6ca00b7        nginx                               "nginx -g 'daemon of…"    About a minute ago   Exited (0) About a minute ago                            affectionate_goldberg
```

### 1.2
``` 
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS                    NAMES
f514761ef215        php:5.6.32-fpm      "docker-php-entrypoi…"   10 months ago       Exited (255) 3 hours ago   9000/tcp                 arkku2_php_1
ce3e554f69a4        mysql:5.7.20        "docker-entrypoint.s…"   10 months ago       Exited (255) 3 hours ago   0.0.0.0:3306->3306/tcp   arkku2_mysql_1
```

### 1.3

```
$ docker run -it devopsdockeruh/pull_exercise
Unable to find image 'devopsdockeruh/pull_exercise:latest' locally
latest: Pulling from devopsdockeruh/pull_exercise
8e402f1a9c57: Pull complete
5e2195587d10: Pull complete
6f595b2fc66d: Pull complete
165f32bf4e94: Pull complete
67c4f504c224: Pull complete
Digest: sha256:7c0635934049afb9ca0481fb6a58b16100f990a0d62c8665b9cfb5c9ada8a99f
Status: Downloaded newer image for devopsdockeruh/pull_exercise:latest
Give me the password: basics
You found the correct password. Secret message is:
"This is the secret message"
```

### 1.4

``` 
$ docker exec -it exercise bash
root@da3de63fe317:/usr/app# tail -f ./logs.txt
Fri, 29 Nov 2019 07:14:36 GMT
Fri, 29 Nov 2019 07:14:39 GMT
Secret message is:
"Docker is easy"
Fri, 29 Nov 2019 07:14:45 GMT
Fri, 29 Nov 2019 07:14:48 GMT
Fri, 29 Nov 2019 07:14:51 GMT
Fri, 29 Nov 2019 07:14:54 GMT
Secret message is:
"Docker is easy"
```

### 1.5

```
docker run -d -it ubuntu:16.04 sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'
```
```
docker exec -it 1c21c2c44b16 bash
```
```
root@bcc931dc04ae:/# apt-get update && apt-get install curl
```
```
$ docker start bcc931dc04ae
$ docker attach bcc931dc04ae
helsinki.fi
Searching..
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://www.helsinki.fi/">here</a>.</p>
</body></html>
```

### 1.6

Dockerfile:
```
FROM devopsdockeruh/overwrite_cmd_exercise
CMD ["--clock"]
```
Terminal:
```
docker run docker-clock
```

### 1.7

Dockerfile:
```
FROM ubuntu:16.04 
WORKDIR /script
ADD /script.sh /
RUN apt-get update 
RUN apt-get install -y curl
CMD ["/bin/bash", "/script.sh"]
```
script&#8203;.&#8203;sh:
```
echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;
```
Terminal:
```
docker run -it curler
```


### 1.8

```
docker run -v $(pwd)/usr/app/logs.txt:/usr/app/logs.txt devopsdockeruh/first_volume_exercise
```
logs.txt:

```
Sun, 01 Dec 2019 13:52:04 GMT
Sun, 01 Dec 2019 13:52:07 GMT
Sun, 01 Dec 2019 13:52:10 GMT
Sun, 01 Dec 2019 13:52:13 GMT
Secret message is:
"Volume bind mount is easy"
```

### 1.9

```
docker run -p 80:80 devopsdockeruh/ports_exercise
```
I open localhost:80 in the browser and I see "Ports configured correctly!!"

### 1.10

Dockerfile:
````
FROM node:8.7.0-alpine 
WORKDIR /frontend-example-docker
ADD . .
RUN npm install
EXPOSE 5000
CMD ["npm", "start"]
````

Terminal:
````
docker build -t exercise-1_10 .
frontend-example-docker $ docker run -it -p 5000:5000 exercise-1_10
````

### 1.11

Dockerfile:
````
FROM node:8.7.0-alpine 
WORKDIR /backend-example-docker
ADD . .
RUN npm install
EXPOSE 8000
CMD ["npm", "start"]
````

Terminal:
````
docker build -t exercise-1_11 .
docker run -p 8000:8000 -v $(pwd)/logs.txt:/backend-example-docker/logs.txt exercise-1_11
````

### 1.12

Frontend Dockerfile:
```
FROM node:8.7.0-alpine 
ENV API_URL=http://localhost:8000
WORKDIR /frontend-example-docker
ADD . .
RUN npm install
EXPOSE 5000
CMD ["npm", "start"]
```
Backend Dockerfile:
```
FROM node:8.7.0-alpine 
ENV FRONT_URL=http://localhost:5000
WORKDIR /backend-example-docker
ADD . .
RUN npm install
EXPOSE 8000
CMD ["npm", "start"]
```
Terminal:
```
docker build -t backend .
docker build -t frontend .
docker run -it -p 8000:8000 backend
docker run -it -p 5000:5000 frontend
```

### 1.13

Dockerfile:
````
FROM openjdk:8 
WORKDIR /spring-example-project
ADD . .
RUN ./mvnw package
EXPOSE 8080
CMD ["java", "-jar", "./target/docker-example-1.1.3.jar"]
````
Terminal:
````
docker build -t java-spring .
docker run -it -p 8080:8080 java-spring
````

### 1.14

Dockerfile:
````
FROM ruby:2.6.0-alpine3.8
WORKDIR /rails-example-project
ADD . .
RUN gem install bundler
RUN apk update \
    && apk add build-base \
       tzdata \
       sqlite-dev \
       nodejs
RUN gen install tzinfo-data
RUN gem install nokogiri
RUN bundle install
RUN rails db:migrate
EXPOSE 3000
CMD ["rails", "s"]
````

Terminal:
```
docker build -t rails-project .
docker run -it -p 3000:3000 rails-project
```

### 1.16

https://docker-exercise-hy.herokuapp.com/

### 1.17

Dockerfile:
```
# Start from an ubuntu environment to make sure that it works on linux, mac and windows
FROM ubuntu:18.04

# install nodejs and npm
RUN apt-get update && apt-get install -y nodejs && apt-get -y install npm

# make the 'app' folder the current working directory
WORKDIR /app

# copy project files and folders to the current working directory (i.e. 'app' folder)
COPY . .

# install project dependencies
RUN npm install

# expose the port 8080
EXPOSE 8080

# default command to execute
CMD [ "npm", "run", "serve"]
```
The repository: https://hub.docker.com/r/eelsie/vue-starter
