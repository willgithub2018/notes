# Docker

[docker install](https://hub.docker.com/editions/community/docker-ce-desktop-windows)  
[documentation](https://docs.docker.com/docker-for-windows/)

## install
```
docker --version
docker run hello-world

docker image ls
docker container ls --all

docker --help
docker container --help
docker container ls --help
docker run --help

# pull ubuntu OS image
docker run --interactive --tty ubuntu bash
```

### docker command
```
hostname
exit
```

### run nginx
```
# Pull and run a Dockerized nginx web server that we name, webserver:
docker run --detach --publish 80:80 --name webserver nginx

#localhost
docker container ls
docker container stop webserver

#remove docker 
docker container rm webserver laughing_kowalevski relaxed_sammet
```
## [101 tutorial](docker.com/101.tutorial)
```
#-d - run the container in detached mode (in the background)
#-p 80:80 - map port 80 of the host to port 80 in the container
#docker/getting-started - the image to use
docker run -dp 80:80 docker/getting-started

docker container ls

docker stop [my_container]
```

## node tutorial
[download demo](http://localhost/assets/app.zip)
```
#add Dockerfile
FROM node:12-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "/app/src/index.js"]

docker build -t getting-started .
docker run -dp 3000:3000 getting-started
```
To test: http://localhost:3000/

## update code example
```
# run a new container
update ~/app/src/static/js/app.js
docker build -t docker-101 .
docker run -dp 3000:3000 docker-101

#port conflict
docker ps
# Swap out <the-container-id> with the ID from docker ps
docker stop <the-container-id>
docker rm <the-container-id>
docker run -dp 3000:3000 docker-101

#alternative
docker rm -f <the-container-id>   
```

## push images to docker registry
[Docker registry](https://hub.docker.com/repository/create)
```
docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname

#example [docker id is d0cker2018]
docker tag docker-101 d0cker2018/dockertutorial
docker push d0cker2018/dockertutorial

#run from Docker hub
#e.g. docker run -dp 3000:3000 YOUR-USER-NAME/101-todo-app

docker run -dp 3000:3000 d0cker2018/dockertutorial
```

### Persist Data

```
# save to data.txt
docker run -d ubuntu bash -c "shuf -i 1-10000 -n 1 -o /data.txt && tail -f /dev/null"
docker exec <container-id> cat /data.txt

# Container Volumnes
docker volume create todo-db
docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started
#add some data adn remove container
docker rm -f <container-id>
docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started
# see data persist

docker volume inspect todo-db

	                        Named Volumes	Bind Mounts
Host Location	            Docker chooses	You control
Mount Example (using -v)	my-volume:/usr/local/data	/path/to/data:/usr/local/data
Populates new volume with container contents	Yes	No
Supports Volume Drivers	    Yes	No
```

### Using Bind Mounts 
```
docker run -dp 3000:3000 \
    -w /app -v $PWD:/app \
    node:12-alpine \
    sh -c "yarn install && yarn run dev"
#    
-dp 3000:3000 - same as before. Run in detached (background) mode and create a port mapping
-w /app - sets the "working directory" or the current directory that the command will run from
node:12-alpine - the image to use. Note that this is the base image for our app from the Dockerfile
sh -c "yarn install && yarn run dev" - the command. We're starting a shell using sh (alpine doesn't have bash) and running yarn install to install all dependencies and then running yarn run dev. If we look in the package.json, we'll see that the dev script is starting nodemon.

docker logs -f <container-id>
```

### MySQL container
#### Starting MySQL
```
docker network create todo-app

docker run -d \
    --network todo-app --network-alias mysql \
    -v todo-mysql-data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=todos \
    mysql:5.7

docker run -d --network todo-app --network-alias mysql -v todo-mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=todos mysql:5.7

docker exec -it <mysql-container-id> mysql -p
#enter secret for mySql password
mysql> SHOW DATABASES;
```
#### Connecting to MySQL
```
docker run -it --network todo-app nicolaka/netshoot
dig mysql

docker logs <container-id>

docker run -dp 3000:3000 -w /app -v $PWD:/app --network todo-app -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=secret -e MYSQL_DB=todos node:12-alpine sh -c "yarn install && yarn run dev"

docker run -dp 3000:3000 -w /app --network -v $PWD:/app --network todo-app -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=secret -e MYSQL_DB=todos node:12-alpine sh -c "yarn install && yarn run dev"

https://docs.docker.com/storage/bind-mounts/

docker logs <containerid>
docker exec -ti <mysql-container-id> mysql -p todos
mysql> select * from todo_items;
```
## Docker Compose
save file in app root folder as docker-compose.yml
```
docker-compose version
```
```
version: "3.7"

services:
  app:
    image: node:12-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:5.7
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment: 
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```
```
# at app docker-compose.yml
docker-compose logs -f 
```

### Best practives
```
docker image history getting-started
docker image history --no-trunc getting-started
```
```
#copy in package.json, install dependencies, copy
FROM node:12-alpine
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install --production
COPY . .
CMD ["node", "/app/src/index.js"]
```
### PROXIES
```
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=b7edf988b2b5
TERM=xterm
HOME=/root
HTTP_PROXY=http://proxy.example.com:3128
http_proxy=http://proxy.example.com:3128
no_proxy=*.local, 169.254/16
```

## Postgres SQL (https://hub.docker.com/_/postgres)
```
docker pull postgres
docker pull postgres:11.5

docker run --name my_postgres -e POSTGRES_PASSWORD=myscretpassword -d postgres:latest
docker ps
docker exec -it my_postgres bash


/usr/bin/psql -U postgres postgres
\q
exit

docker rm -f my_postgres
```
