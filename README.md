# Docker Commands
This is a tutorial on all the major Docker commands you need to know to start using Docker.I used freeCodeCamps's
Docker tutorial for this cheatsheet 


## run 

This command will start a container from a image.A image is a template for the configuration of the container.

```
docker run "container"

```
After runnning this command , whatever container was listed as "container" will be deployed.

When running a image , it will default to the latest version of it.If you want to use previous version , you can use tags 

```
docker run redis
docker run redis:"version"

```
When you have a interactive shell running on your container ,simply running it will not prompt you to enter any input.To fix this
we use the options -i to allow for inputs , and -t to start a psuedo terminal.There will be no prompt without the -t because the
interactive program is in the containers terminal.Using -t we can see it in our terminal 

```
docker run -it "image"

```
So when you launch for example a web application using docker on your local machine , it will have a ip address associated with it.This
ip address will be a internal ip address and other people cant access it.But they can access the ip address of the docker host.Using this ,
we can have service ports such as sql,http,https open on the host machine , and use port mapping to mape these ports to ports that are open on
your containers

```
docker run -p 80:5000 "container"
```
Now all traffic going to the host on port 80 will be directed to the container with port 5000 open 


When you delete a container,all data on that container is also gone.They have a ephemeral container.You can use volume mapping
to mount whatever data is on your contaianer to a external storage on the docker host.Now when you delete the cotainer , the data
is still there

```
docker run -v "path/of/storage:/path/of/storage/on/container"
```
## inspect command
This command gives you more specific information about the container you are running.More information than the ps command and
gives your data in a json format

```
docker inspect "container"

```
You can view the logs of a container that you are running in the background when you did the -d command(allows you to continue use 
the terminal while it is running , think of python server where you had to ctrl c out of it and close the server to use the terminal 
again)
```
docker logs "container"

```
## 


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

## enviorment variables

When you have a application with variables that you might want to change in the future,instead of hard coding
the variable you can use a enviormentent variable.For example you have an application that has white background.When you 
want to change the background to black , you would have to change the code , instead you can specify the background color
as a enviormental variable , and specify the color when you run it.

```
docker run -e "ENVIORMENTAL_VARIABLE=..."

```
you can inspect the container to see what enviormental variable there is in the container

# Create Images

## Docker files
This files contains all the commands needed to create a local image.Create your own image.All docker files must start with a
from.The from specifies what images you are basing your image off of.It can be a operating system , or other created images.
Creates instruction on how the image should be build
You will use the build command to build the image and you can use the push command to upload this image into docker hub.

The docker file will be created in layers.The first layer would be the base layer(os) then the apt packages such as pythons, then 
the dependencies/librarys

```
From ubuntu -- builds the base os 

Run apt-get update && apt-get -y install python -- builds the apps 
Run pip install flask flask-mysql -- builds the librarys

COPY . /opt/source-code -- copy source code that is in local directory onto the container

ENTRYPOINT FLASK_APP=/opt/source-code/app.py run

```
When building the image , since things are built out in layers , when a particular layer fails , you just have to fix that layer , you 
dont have to go through the process again 






















`







