

In this demo, we will go a step forward with writing Dockerfile and we'll explore configuration instructions.

Again, ee are in `S2` directory which contains individual directory for every demo.

Let's navigate to the directory called `D2`. There we go!

```bash
pwd
```
```
/home/franck/workspace/learning/docker/udemy/CC_Docker/S2/D2
```

As you can see, there is a `Dockerfile` already present in this directory.

Let's open it with nano. As you can see, this Dockerfile also has a base image of **Ubuntu 16.04** mentioned using `FROM` instruction as described in previous demo.

```Dockerfile

FROM ubuntu:16.04

LABEL Creator: "Cerulean Canvas"

RUN apt-get update && apt-get install -y curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir /home/Codes

ENV USER Cerulean-Canvas 
ENV SHELL /bin/bash
ENV LOGNAME Cerulean-Canvas

CMD ["bash"]

```

But this time, we skipped using `ARG` instruction and directly provided the version number.

Now we `RUN` and `ENV` which are configuration instructions. Although they are not the only entries in the list of configuration instructions.

But these are the ones that we will cover in current demo.

Let's go through them one by one.


# RUN

`RUN` asks Docker to execute the command mentioned with it on top of the base image and the results are committed as a separate layer on top of the base image layer.

Here, we have more than one mentions of `RUN` and each one creates its own separate layer. With the **first `RUN` instruction**, we have provided the commands to :
1. Update the OS
2. Install curl
3. And clean up later on

```Dockerfile
RUN apt-get update && apt-get install -y curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
```

Whereas second RUN, simply makes a directory named "Codes" under "home" directory.

```Dockerfile
RUN mkdir /home/Codes
```

Don't confuse it with your host machine's home directory though here we are talking about **base image OS home directory** and the codes will be created on that base image not on our host machine.


# ENV

Then we have used `ENV` which is another configuration instruction.

It does what its name suggests. it sets up environmental variables.

```Dockerfile
ENV USER Cerulean-Canvas 
ENV SHELL /bin/bash
ENV LOGNAME Cerulean-Canvas
```

## USER, SHELL & LOGNAME

We have used it 3 times to set `USER`, `SHELL`, and `LOGNAME` environment variables. Just like the previous demo, we have used `CMD` but we will go into that later.

Again, we will use the `docker build` command to build this image, but this time we will tag it as `img_run-env` to separate it from the previous image.

```bash
sudo docker build -t img_run-env .

sudo docker build -t img_run-env - < Dockerfile.txt
```

```
[+] Building 16.6s (7/7) FINISHED                                                                                                                     docker:default
 => [internal] load build definition from Dockerfile                                                                                                            0.0s
 => => transferring dockerfile: 313B                                                                                                                            0.0s
 => [internal] load .dockerignore                                                                                                                               0.0s
 => => transferring context: 2B                                                                                                                                 0.0s
 => [internal] load metadata for docker.io/library/ubuntu:16.04                                                                                                 0.8s
 => CACHED [1/3] FROM docker.io/library/ubuntu:16.04@sha256:1f1a2d56de1d604801a9671f301190704c25d604a416f59e03c04f5c6ffee0d6                                    0.0s
 => [2/3] RUN apt-get update && apt-get install -y curl     && apt-get clean     && rm -rf /var/lib/apt/lists/*                                                15.3s
 => [3/3] RUN mkdir /home/Codes                                                                                                                                 0.3s
 => exporting to image                                                                                                                                          0.1s 
 => => exporting layers                                                                                                                                         0.1s 
 => => writing image sha256:209d96c57387f2b63a7597c14cea73d56ece0ce8d9e12184d2f412b9de0a8e2a                                                                    0.0s 
 => => naming to docker.io/library/img_run-env                                                  
```

As you can see, in this build :

The **first step** is directly involving setting up the base image since we have skipped using `ARG` instruction.

**Step two** will perform all of the commands used in first `RUN` instruction and will perform the commands of second `RUN` instruction which is making a directory.

**Step 4, 5, and 6** will set environmental variables as mentioned in the Dockerfile.

And the superfast **step 7** will get our image ready to run.
```bash
CMD ["bash"]
```

Let's list out our available images with docker images command. These images are the ones currently available on the host. Our top image is the `img_run-env` image.

```bash
sudo docker images
REPOSITORY    TAG       IMAGE ID       CREATED             SIZE
img_run-env   latest    209d96c57387   54 seconds ago      151MB
img_from      latest    663569db8d63   About an hour ago   162MB
nginx         latest    a8758716bb6a   3 months ago        187MB
hello-world   latest    d2c94e258dcb   9 months ago        13.3kB
```

Now, let's go one step further, and run this image as container with `docker run -itd --name cont_run-env img_run-env` command.
- The `-itd` represents **interactive**, **teletype** enabled, and **detached** respectively.
- We are naming the to be **running container** `cont_run-env` and the **target image** is `img_run-env` which we just created.

```bash
sudo docker run -itd --name cont_run-env img_run-env
```

The command was successful and we have just received the **unique container ID** provided by Docker for our container.

```
184d803c366ab94f169ff39895e602c2f31f08506739ed62f7b0a710f59fbc56
```

Here, we have **three containers** running among which **first** is the one which we ran recently.

```bash
sudo docker ps -a
```
```
CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS                    PORTS     NAMES
184d803c366a   img_run-env    "bash"                   About a minute ago   Up About a minute                   cont_run-env
1c9d3fc97857   nginx:latest   "/docker-entrypoint.…"   44 hours ago         Exited (0) 44 hours ago             web-server-nginx
44c5e486a49d   hello-world    "/hello"                 45 hours ago         Exited (0) 45 hours ago             clever_solomon
```

It is up means running for about **one minute** and it is **running** the `bash` command.

Now let's execute our containers.

## bash command

Here, the `bash` command and the process was running in background due to the detached flag set while running the containers.

```bash
sudo docker exec -it cont_run-env bash
```

Now, we are bringing it forward. As you can see, now we are in the root directory of our cont_run-env container.

Let's list out the directories here.

```
root@184d803c366a:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

**Yes!**

The structure looks similar to a regular Linux instance.

Now let's verify the environmental variables which we had set with ENV instruction while writing the Dockerfile.

```bash
echo $USER
echo $SHELL
echo $LOGNAME
```
```
Cerulean-Canvas
/bin/bash
Cerulean-Canvas
```

As you can see, the `USER`, `SHELL`, and `LOGNAME` variables are just as we had set them up.

Now, let's navigate to home directly. As we list it out, we can also verify the creation of the "codes" directory which was supposed to be created from our RUN instruction of the Dockerfile.

```bash
cd /home/Codes/
```

Finally, we can get back to our host environment by exiting the container using simple exit command.

```bash
exit
```
