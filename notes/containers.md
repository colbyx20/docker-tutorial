Goal: Lean how to run, manage, and delete containers
1. Docker run and it's flags
2. docker ps, stop, start, rm
3. interactive mode and detached mode 

Docker CLI
* `docker [command] [option] [imagename] [arguments]
Common used CLI commands

| Command            | Description                       |
| ------------------ | --------------------------------- |
| docker run         | Run a container                   |
| docker ps          | List running containers           |
| docker stop, start | Stop or start container           |
| docker rm, rmi     | Remove container or images        |
| docker exec -it    | Run command inside container      |
| docker logs        | View continer logs                |
| docker build -t    | Build an image from a Dockerfile  |
| docker pull, push  | Download/upload from/to registery |
Important Docker Run Flags

| Flag (options)    | Description                                |
| ----------------- | ------------------------------------------ |
| --rm              | Automatically remove container when exit   |
| --name name       | Assign a name to the container             |
| -d                | detatched mode (run in background)         |
| -it               | interactive + TTY (for sells/bash)         |
| -v volume         | Mount aa volume or host directory          |
| -p host:container | Publish container ports to host            |
| --env/-e          | Set environment variables inside container |
| --network         | connect container to a specific network    |
| --entrypoint      | override default entrypoint                |


```bash
# Create your first container
docker run hello-world

# optional - add a custom name to your container
docker run --name=mycontainer hello-world

# View running containers
docker ps

# view all containers running and stopped
docker ps -a
```

Example commands
```bash
# run alpine container setting env and listing env to stdout
docker run -e APP_MODE=production alpine env

docker run -e USER=admin -e DEBUG=true alpine env

# with file .env
APP_PORT=8080
APP_MODE=debug

docker run --env-file .env myapp

# Passing command-line arguments
docker run ubuntu echo "Hello from Ubuntu"

# Override entrypoint - run ls -l /etc
docker run --entrypoint ls ubuntu -l /etc

# Execute bash
docker run -it ubuntu bash

# Launch web server
docker run -d --rm --name web -p 8080:80 nginx

# set env and print
docker run --rm -e MY_NAME=Dev dockerina/env-printer

# Override CMD & Entrypoint
docker run ubuntu echo "custom command"
docker run --entrypoint sleep ubuntu 10
```


## **Lab  Playing with your first container**

1. Create an nginx container and give the container a name
     `docker run --rm --name web nginx:latest`
     * Notice - you terminal sessions is now outputting the logs of nginx
	     * If you wish to run this container in the background add the -d flag
	     * `docker run -d --rm --name web nginx:latest`
2. Open up another terminal and list all running docker containers
		`docker ps`
3. Grab Ipaddress of this container
		`docker inspect [containerID] `
		`docker inspect [containerID] | grep -i ipaddress`
4. Kill the container 
	1. If you ran with out -d option - `CTRL+c`
	2. with -d option - `docker stop [containerID]`

## **Expose container to host**
1. `docker run -d -p 3000:80 --name web nginx:latest`
	* In web browser go to localhost:3000
2. Stop the Docker container verify if localhost:3000 still works
	* `docker stop [containerID]`
3. Start container again and verify localhost:3000 is working
	* `docker start [containerID]

## Shell commands and interactive shell
`docker exec [containerID | containerName] command`
1. `docker exec web env`
	* list out all env variables to stdout
2. `docker exec -it web /bin/bash
	* Notice shell output - root@containerHost
	* `docker inspect [containerName] | grep -i hostname`
3. Now you're inside nginx container - this is based of ubuntu
4. Update apt and install vim
	* `apt update -y && apt install vim`
5. Update the index.html page then refresh browser
```bash
echo "<h1>Updating Page</h1>" >> /usr/share/nginx/html/index.html
```

