

Let's navigate to the `D6` directory and list out all of the contents of it.

```bash
cd CC_Docker/S2/D6
```

We have a Dockerfile for this demo available here. Open the Dockerfile in the text editor.

```Dockerfile
FROM ubuntu:16.04

RUN apt-get update && apt-get install nginx -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

As we can see, it contains four Docker instructions.

`FROM` instruction will set the base image ubuntu:16.04 as its base image for this docker image.

Next instruction is `RUN` which will update and install **nginx** on **ubuntu:16.04** base image.

We will change subcommands of `RUN` instruction with logical AND operator (**&**) which means in order to run second subcommand, first command should be a success.

Here, If we consider the sequence, `apt-get update` of the base image should be a success, in order to install **nginx**. After nginx installation `apt-get remove` and `rm -rf /var/lib/apt/list` will clear up local depositories of retrieve packages.

Next instruction `EXPOSE` is a type of documentation which will inform Docker about the **port** on which the container is **listening**.

Keep in mind! It does not publish the port but it fills the gap between the docker image builder and the person who runs the container. We documented with `EXPOSE` instruction that this nginx container will listen on **port 80**.

`CMD` instruction will make **nginx** application **run in foreground** by turning off nginx as a demon process. Exit from Dockerfile.

```bash
sudo docker build -t img_expose .
```
```bash
sudo docker build -t img_expose - < Dockerfile.txt
```


Build the docker image docker build command from the `Dockerfile` available in the present directory and tag it as `img_expose`. The build context is sent to docker daemon. As we already have ubuntu 16.04 image in local docker storage, docker daemon does not download it again.

It is cached! In **step 2**, chain RUN Instruction is being executed one by one.

**First**, it will update the package index of the base image ubuntu 16.04. After successfully updating the image, nginx will be installed on the base image, and at the end local repos of retrieved packages will be cleared up.

**Step 3**, is to expose the port 80 of the container in order to inform docker that nginx app will listen on port 80.

The **last step**, is setting up the default command `CMD`, which will set the nginx app as the foreground process in this container.

Our image has been successfully built and tagged as `img_expose`.

Let's list out all the images in our local docker storage.

```bash
sudo docker images
```
```
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE                                                                                                         
img_expose    latest    bb2475302bb4   28 seconds ago   191MB
img_run-env   latest    209d96c57387   29 hours ago     151MB
img_from      latest    663569db8d63   30 hours ago     162MB
nginx         latest    a8758716bb6a   3 months ago     187MB
hello-world   latest    d2c94e258dcb   9 months ago     13.3kB
```
There we go!

`img_expose` has been successfully created and stored on docker.




Let's run a container based on `img_expose` image

Type :

```bash
sudo docker run -itd --rm --name cont_expose -p 8080:80 img_expose
```

`rm` flag will automatically remove the container once it has stopped.

Follow it with container name `cont_expose` followed by `-p 8080:80` which means map the container's port 80 with host's port 8080, in order to access nginx service. And finally, give the image name which is `img_expose`.

Press enter and we got the container ID! Let's list out all the running and stopped containers with docker ps -a command.

Our `cont_expose` is up and running for 7 seconds, the containers port 80 has been mapped on port 8080 of the host so that we can access the nginx web server on our favourite web browser.

Now, go to your favorite web browser, mine is Chrome, and type [http://localhost:8080](http://localhost:8080) in the address bar.

Press enter and we can see the default home page of nginx web server.

Then
```bash
sudo docker ps -a
```
```
CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS                      PORTS                                   NAMES
e6b38b1c9e2c   img_expose     "nginx -g 'daemon of…"   About a minute ago   Up About a minute           0.0.0.0:8080->80/tcp, :::8080->80/tcp   cont_expose
184d803c366a   img_run-env    "bash"                   28 hours ago         Exited (255) 26 hours ago                                           cont_run-env
1c9d3fc97857   nginx:latest   "/docker-entrypoint.…"   3 days ago           Exited (0) 3 days ago                                               web-server-nginx
44c5e486a49d   hello-world    "/hello"                 3 days ago           Exited (0) 3 days ago                                               clever_solomon
```
```bash
sudo docker stop e6b38b1c9e2c
```
