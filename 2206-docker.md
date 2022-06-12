## Docker Image
- Image is a template for creating an environment of your Choice
- Snapshot
- Has everything need to run your Apps
- OS, Software, App Code
## Container
- Running instance of an Image
- IMAGE -- run --> CONTAINER

## 1st docker image
- docker pull nginx
- docker images
- docker run nginx:latest
- docker -d run nginx:latest
- docker -d run -p 8080:80 nginx:latest
- docker -d run -p 8080:80 -p 3000:80 nginx:latest
- docker -d run --name website -p 8080:80 -p 3000:80 nginx:latest
- ----
- docker ps
- docker container ls
- docker stop <id>|<name>
- docker start <id>|<name>
- ----
- docker ps -a
- docker rm <id>
- docker rm  $(docker ps -aq)
- ----
> export FORMAT="ID\t{{.ID}}\nNAME\t{{.Names}}\nImage\t{{.Image}}\nPORTS\t{{.Ports}}\nCOMMAND\t{{.Command}}\nCREATED\t{{.CreatedAt}}\nSTATUS\t{{.Status}}\n"
- docker ps --format=$FORMAT
- ----
>  docker run --name website -v $(pwd):/usr/share/nginx/html:ro -d -p 8080:80 nginx:latest
- 命令行进入container shell
> docker exec -it website bash //enter container
> root@056e3384e057:/#
- ----
- volumes share between 2 containers
> docker run --name website-copy -d --volumes-from website -p 8081:80 nginx
- ----
- Dockerfile
    - Build owner images 
    - Dockerfile -- create --> Image -- run --> Container 

- ----
- Docker Registries - aka Docker image repository
    - Highly scalable server side application that stores and lets you distribute Docker image
    - Used in your CD/CI Pipeline
    - Run you applications
    - 2 types Docker Registry: Private / Public
        - Docker Hub
        - quay.io
        - Amazon ECR
- Push image to Registry: hub.docker.com    
    - create a hub.docker.com account
    - in hub.docker.com website, create a public repository named [website]
     docker tag wehelpwe-website:2 wehelpwe/website:2
    - docker tag wehelpwe-website:1 wehelpwe/website:1
    - docker login // to login from cli
    - docker push wehelpwe/website:2
    - docker push wehelpwe/website:1
- docker inspect user-service-api:alpine
- docker logs c54e05a9a519 -f // follow 
- docker exec -it user-api  /bin/sh   // into a docker shell, interactive and tty
- docker exec --help

        Usage:  docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

        Run a command in a running container
        Options:
          -d, --detach               Detached mode: run command in the background
              --detach-keys string   Override the key sequence for detaching a container
          -e, --env list             Set environment variables
              --env-file list        Read in a file of environment variables
          -i, --interactive          Keep STDIN open even if not attached
              --privileged           Give extended privileges to the command
          -t, --tty                  Allocate a pseudo-TTY
          -u, --user string          Username or UID (format: <name|uid>[:<group|gid>])
          -w, --workdir string       Working directory inside the container
