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
we can have services such as sql,http,https open on the host machine , and use port mapping to mape these ports to ports that are open on
your containers

```
docker run -p 80:5000 "container"
```
Now all traffic going to the host on port 80 will be directed to the container with port 5000 open 


When you delete a container,all data on that container is also gone.They have a ephemeral container.You can use volume mapping
to mount whatever data is on your contaianer to a external storage on the docker host.Now when you delete the cotainer , the data
is still there.It doesnt matter that the containers share the same port because they have different ip addresses.The host cant have the same
port mapped to different containers.
You have two containers.You have two ports open on the host.Ythe containers even though they have the same port open.Useres will be routed to
the container based on what port they connected to on the host.

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
## CMD VS Entry Point

Or instead of running the sleep command everytime you start this container , you can add this command to the dockerfile of that container.
Now everytime that container is launched , they will run that command.

```
cmd sleep5
```
But now if we want to change the value of a cmd command , we can use ENTRYPOINT.Instead of hard coding the value of how long the container
should be sleeping we can use entry-point to specify the command,and we will specify the parameters of that command when we use the run command
```
ENTRYPOINT['SLEEP']
docker run ubuntu 10
```
Now when we run the docker command it will that in the value 10 as how long it will sleep
We can set a default value if we dont specify it ourself when we launch the container by combining both commands

```
ENTRYPOINT["SLEEP"]
CMD["5"]
```
Now when you dont specify a value,cmd will fill it for you.
You can also override a entrypoint by specifying it when you run your container

```
docker run --entrypoint "command" "image" "parameter"
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


# NETWORKING

## Bridge network
The default network you when you use docker.All contaianers share the same subnet in the docker host and can communicate with each other, and the 
host can map traffic to these containers by using port mapping.

## None
Your containers have no network.Nobody can access them externally 

## Host 
The container shares the same ip as the host.External users can connect to this contaianer without using port mapping but now you cant have 
multiple containers sharing the same port because they are all sharing the host ip 

## user-defined networks
we can create our own networks within docker.We can segment different containers from talking to each other
```
docker network ls
```
Use this command to see all the networks in docker 

when we use the inspect command we can also find the network ip and subnet of that container.

## DNS
docker has a built in dns so that way you can connect to other containers on the same network by using their name 
instead of their ip since their ip might change on reboot



# Storage
## File system
When you instaall docker , docker creates a folder structure in /var/lib/docker.In the directory docker , it would contain directory 
to auf,container,image,volume.All the data about images,containers etc.. will be stored here.

## layers
the image layer is read only , and will always be there even when you delete the container.When you dleete the container all data will be 
deleted as well.The image layer is below the container layer.Image layer are read onlu , container layer is read and write.
But if you want to make changes to a file in the image layer, you can make a copy of that file and move it to the container layer.This is 
called copy-on-write

## volumes
to create a volume we would use this command
```
docker volume create data_volume
```
and now when we use a run command to deploy a container we would specify the volume we just created as a place to store the data
```
docker run -v data_volume:/var/lib/mysql mysql
```
If you use the run command and specify a volume that has not been created yet , docker will create one for you.
This is called volume mounting.Which means all data are stored on the /var/lib/docker/volumes folder.If you want to specify 
to store this data somewhere else , you can provide the full path to it when you use the run command.

```
docker run -v /data/mysql
```
Volume mount mounts the data ti the dockers volumes folder and bind mount,mounts the data anywhere on the host.
The new way is to use the --mount option

```
doocker run \
--mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql
```

# Compose
Use a yaml file instead of typing out commands one by one.Instead of running the run command 5 times , you can create a yaml file
and just run the yaml file.

To link together multiple containers , you can use the link command when you create the container.

You can use the command
```
docker-compose uo
```
This will bring up the yaml file, create it/launch it.You can only launch images in docker.Docker have a lot of images but if we want to
launch our own application that means we would have to create our own image.To create our own image we used the build command.Push if you
want to push it to docker hub.So in the yaml file , we can use the "build:" command to build out image and deploy it.You would build out 
the docker file.The docker file will contain instructions on the os and all the dependencies needed.It will also contain a command to
copy the source-code of you application into the image.

There have been many different versions of Docker.

In version 2 or up you have to specify the version of your yaml file.Using this version you dont need to specify links to connect your
containers.You can also use the depends on feature.This will let docker know what containers it need to use first.Version3 adds
support for docker swarms.You can also use yaml to segment the network.Divide the network up into parts.

# Registry
Registry is where all the images are stored.The typical format for where to pull the image from is 
image:user/image so when you do image:nginx since you didnt specify the user , it defaults to whatever you put for image.The registry is
what stores all the user/image sos registry/usr/image.Your registry can be from docker , so docker hub , or it can be from google with
many kubernetes images.If you dont want you images to be uploaded to these public registry , you can create a private
refistry and use it internally.

you would have to first login to the private registry
```
docker login private-registry,io
```
and then run the image specifying the full path.
```
docker run private-registry.io/apps/internal-app
```
To deploy your own registry , there is a dockers image for that.You can run the command
```
docker run -d -p 5000:5000 --name registry registry:2
```
then tag your image with the url of the registry and since the registry is running on your local host you can do 

```
docker image tag my-image localhost:5000/my-image
```
Tags help you push and pull images from registry.You can have one tag on multiple images as well as a image that have multiple tags
on it.
```
docker push localhost:5000/my-image
```

# Engine
A docker engine is the host that the docker is running on.There are three layers , a docker daemon that runs in the background and
processes and manages the images,containers,network etc...Then there is the rest API server , which will communicate with the with the 
daemon for instructions.Then there is the cli.The docker cli doesnt need to be on the same host.Can be use on a different host 
and use the command
```
docker -H=remote-docker-engine:2375
```
to connect remotely






















