# Docker Commands
This is a tutorial on all the major Docker commands you need to know to start using Docker.I used freeCodeCamps's
Docker tutorial for this cheatsheet 


## run 

This command will start a container from a image.A image is a template for the configuration of the container.

```
docker run "container"

```
After runnning this command , whatever container was listed as "container" will be deployed.


## ps 

This command will list all containers that are currently running 

```
docker ps

```

It will also give additional information such as a container id , the image ,when it was created, how long has it been running , 
the ports that are open and the name of it.

```
docker ps -a

```

If you want to have all containers that were deployed no matter if it is running or not you can run this command.


## Stop 


This command will stop a container that is running. You need to specify either the name or the container id.

```
docker stop "names/container id"
```

## rm 

This command will remove a container permantly, it will not show up even if you run ps -a

```
docker rm "names/container id"

```
## images 

This command will list all the images that is local.

```
docker images

```

## rmi

This command will delete the images that you have locally 

```
docker rmi "image" 

```
## pull 

Similar to the run command.The run command will run a image , and if it is not found locally , it will pull the image
locally and run it.If you just want to pull a image without running it first , use the pull command 


```
docker pull "image"

```

## Append command 

When you run a container, you are running processes on that container as well.The container lives as long as the processes
that are living on it.Containers are not meant to be ran as a OS.iT IS meant for processes and applications.
If you run something like docker run ubuntu.The container will not be running. But if you want the container to continue to 
be up even with nothing running on it you can run a command like this 

```
docker run ubuntu sleep 5

```
## exec 

You use the exec command when you want to run a command on a running container 

```
docker exec "container name/id" echo "hello world"

```

## run - attach and detach
run this command when you want your docker web application container to run int the background.

```
docker run -d "web app "

```
















`







