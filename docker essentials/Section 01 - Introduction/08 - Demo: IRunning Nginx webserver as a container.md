
```bash
sudo docker image pull nginx:latest

sudo docker images
```

```
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
nginx         latest    a8758716bb6a   3 months ago   187MB
hello-world   latest    d2c94e258dcb   9 months ago   13.3kB
```

```bash
sudo docker container run -itd --name web-server-nginx -p 8080:80 nginx:latest
#  -i = keep STDIN open even if not attached (interactive)
#  -t = pseudo-terminal, allocating a terminal (TTY)
#  -d =  Run the container in detached mode (in the background)
#  --name=web-server-nginx : Assign a name to the container, `web-server-nginx`
#  -p 8080:80 : Map port 80 on the host to port 80 in the container
# Note that you can’t use -t and -i at the same time unless stdin is enabled (-i).

# List running containers
docker ps

# Check the running containers with details (--all shows all, not just running)
docker ps --all

```
```
1c9d3fc97857   nginx:latest   "/docker-entrypoint.…"   3 minutes ago    Up 3 minutes                0.0.0.0:8080->80/tcp, :::8080->80/tcp   web-server-nginx
44c5e486a49d   hello-world    "/hello"                 43 minutes ago   Exited (0) 43 minutes ago                                           clever_solomon
```

`web-server-nginx` --> `1c9d3fc97857`

```bash


# Get the IP address of your Docker machine
ip=$(ifconfig | grep inet | awk '{ print $2 }'| sed 's/addr://g')
echo "Your Docker Machine IP Address is ${ip}"

# Open another terminal and access the Nginx server from your host OS browser at http://<your_IP>:8080

sudo docker stop 1c9d3fc97857

```



## Transcription

In first demo, we had installed and run nginx on Ubuntu 16.04 locally. In the demo

after that we installed docker.

You might find a pattern here. And you might have been able to figure out that in this demo, we are going

to run nginx as a "Docker container". Unlike hello-world container,

We will do this in a bit more elaborate way.

Let's start with pulling an image called "nginx:latest" from docker hub's nginx repository by running the

command "docker image pull nginx:latest"

This will download or pull an image called nginx with latest tag which can later be run as a container.

Let's see if we got an image. Run "docker images"

command to show the list of images.

And here we go!

We have two images. First, is hello-world which we used in the last demo.

And second, is nginx which we are using in this demo.

Both of them have tag called "latest" and they have different sizes.

Now, let's run this image as a container using "docker containers run" command followed by "itd flag" and name

our container "web-server-nginx", with -p command

we are mapping the "port 8080" of our local machine to container's "port 80".

And finally, we are mentioning the image name "nginx:latest" which we have just pulled recently.

What we got is a container ID of the nginx container.

I know, all of these terminology sounds pretty new and pretty abrupt.

But, don't worry. In this demo, our only purpose is to run nginx successfully.

We will go through all of these items in sufficient details when the time arrives.

Let's verify that our container is running by running the command

"docker ps -a"

And as you can see, web-server-nginx container is running which built upon image called nginx:

latest.

Finally, let's see the output of this container by going to the web browser and opening our local host

port 8080.

And it worked successfully!