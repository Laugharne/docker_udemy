

In this demo, we are going to demonstrate the use of volumes which we have discussed in the theory.

Let's start with creating the volume which we had deleted in the last demo, which is `vol-ubuntu`. Do it by running a container from ubuntu image and call it `cont-ubuntu`.

```bash
docker run -itd --name cont-ubuntu --volume vol-ubuntu:/var/log ubuntu:latest
```
```
00dbe7ed76ad77ac7f450a7c994f28141fe2185c2ff34284ab07133c07b17c21
```

Let's see if both the volume **vol-ubuntu** and the container **cont-ubuntu** are available.

```bash
docker volume ls
```
```
DRIVER    VOLUME NAME
local     vol-busybox
local     vol-ubuntu
```

```bash
docker ps -a
```
```
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS                      PORTS                                     NAMES
00dbe7ed76ad   ubuntu:latest    "/bin/bash"              2 minutes ago   Up 2 minutes                                                          cont-ubuntu
095e28c7829a   img_expose       "nginx -g 'daemon of…"   25 hours ago    Exited (255) 24 hours ago   0.0.0.0:32768->80/tcp, :::32768->80/tcp   cont_nginx-A
bf877f3ab875   img_expose       "nginx -g 'daemon of…"   25 hours ago    Exited (255) 24 hours ago   0.0.0.0:8080->80/tcp, :::8080->80/tcp     cont_nginx
0684e722b0ab   busybox:latest   "sh"                     26 hours ago    Exited (255) 24 hours ago                                             cc-busybox-A
184d803c366a   img_run-env      "bash"                   4 days ago      Exited (255) 4 days ago                                               cont_run-env
1c9d3fc97857   nginx:latest     "/docker-entrypoint.…"   6 days ago      Exited (0) 6 days ago                                                 web-server-nginx
44c5e486a49d   hello-world      "/hello"                 6 days ago      Exited (0) 6 days ago                                                 clever_solomon
```


Again to remind you, we can always check the container using docker container inspect command and find information about volume by formatting its output.

```bash
docker container inspect --format "{{json .Mounts}}" cont-ubuntu | python3 -m json.tool
```
```json
[
    {
        "Type": "volume",
        "Name": "vol-ubuntu",
        "Source": "/var/snap/docker/common/var-lib-docker/volumes/vol-ubuntu/_data",
        "Destination": "/var/log",
        "Driver": "local",
        "Mode": "z",
        "RW": true,
        "Propagation": ""
    }
]
```
As you can see, the container called **cont-ubuntu** has volume **vol-ubuntu** attached to it.

Now, let's execute our container and run bash command on it.

```bash
docker exec -it cont-ubuntu bash
```

You can notice that we are not executing it as a demon container which means that once this command succeeds, we will jump right into the terminal of our container.

Right now, this container is in its default state which means that even if we delete it and spin it up again, nothing will change!

So, let's make a few changes to it, which will be reflected in its read-write topmost layer.

And if we delete the container then, the changes we have made now would be lost.

The action can be pretty simple here.

We don't need to do something so heavy.


Even a simple act of just **updating the OS** can create enough changes to be recognized.

So, let's update this ubuntu by typing `apt-get update` command.

```bash
apt-get update
```

Once it is updated, let's change our working directly to `/var/log`. As you may have guessed, this is the directory where ubuntu is keeping its logs.

Let's list out the available files. And we have a lot of log files here.

```bash
ls
```
```
alternatives.log  apt  bootstrap.log  btmp  dpkg.log  faillog  lastlog  wtmp
```

The purpose of doing so is to make sure that once we stop the container, we should be able to see the same files as backup on host machine.

And the reason for that is when we created this container, we had mount this directory (`/var/log`) to our host using the volume **vol-ubuntu**. let's exit the process and stop the container.
```bash
docker stop cont-ubuntu
```

Now, let's have the root privileges on our host machine.
```bash
sudo su -
```

And as you can see, we are on the same working directory just with root privileges.

Now as we have seen in the theory section of volumes, docker stores the backup of volume data under `/var/lib/docker/volumes` directory.
```bash
cd /var/lib/docker/volumes
```

So, let's navigate to it. And let's list out the content in this directory.

As you can see, we have directories of all of the volumes created by local volume driver.

Now, let's navigate to vol-ubuntu to see if that changes in the log file are reflected.

Once we are in vol-ubuntu directory, let's see its contents. and what we have is a data directory. Once we navigate to that and list its contents, what we see is a long list of log files. Which means that the mounting of volume with the container was successful!

So, this is how we mount a volume to a container and create a backup of its data to host using local volume driver.