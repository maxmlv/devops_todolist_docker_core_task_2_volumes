# Docker Core / Containerizing an App / Docker by practice | ToDo App

## Docker Hub Repository

MySQL database server image on Docker Hub:

[https://hub.docker.com/repository/docker/maxmlv/mysql-local](https://hub.docker.com/repository/docker/maxmlv/mysql-local)

The application image on Docker Hub:

[https://hub.docker.com/repository/docker/maxmlv/todoapp](https://hub.docker.com/repository/docker/maxmlv/todoapp)

## Set-up MySQL database

Create docker volume `mysql-data`:

```bash
docker volume create mysql-data
```

Pull the `mysql-local` image:

```bash
docker pull maxmlv/mysql-local:1.0.0
```

Run the `mysql-local` container with volume attached:

```bash
docker run -d --name mysql-local -v mysql-data:/var/lib/mysql -p 3306:3306 maxmlv/mysql-local:1.0.0 
```

## Build the App Image

### Clone the application repository

```bash
git clone https://github.com/maxmlv/devops_todolist_docker_core_task_2_volumes.git
cd devops_todolist_docker_core_task_2_volumes
```

### Connect the App to MySQL

Before building the app image, get the running MySQL container's IP address:

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mysql-local
```

Update `todolist/settings.py` with this IP in the `DATABASES['default']['HOST']` field, then proceed to build the app image below.

The default value of IP address in `settings.py` is `172.17.0.2`

### Build the image locally:

```bash
docker build -t todoapp:2.0.0 .
```

## Run the application from the Docker Hub image (or local)

### Pull the image

```bash
docker pull maxmlv/todoapp:2.0.0
```

### Run the Container

```bash
docker run -d --name todoapp -p 8080:8080 todoapp:2.0.0
```

- `-d` — run in detached mode (in the background)
- `-p 8080:8080` — map container port 8080 to host port 8080
- `--name todoapp` — name the running container

## Our application is up and running!

![Project Screenshot](./screenshots/Screenshot%202026-06-20%20220018.png)

## Access the Application

Once the container is running, open your browser and go to:

[http://localhost:8080](http://localhost:8080)

## Stopping the Containers

```bash
docker stop todoapp mysql-local
docker rm todoapp mysql-local
```