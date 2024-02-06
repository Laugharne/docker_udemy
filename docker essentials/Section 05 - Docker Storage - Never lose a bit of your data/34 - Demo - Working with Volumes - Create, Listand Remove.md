

In this demo, we are going to create a volume using docker command line.

Let's type the command "docker volume create" followed by the name of the volume.

Here, we are naming the volume "vol-busybox".

```bash
docker volume create vol-busybox
```
```
vol-busybox
```
Once the command succeeds, we get the name of the volume as a nod of being created. Before we do anything to this created volume, let's create another one.

```bash
docker run -d --volume vol-ubuntu:/tmp ubuntu
```
```
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
57c139bbda7e: Pull complete 
Digest: sha256:e9569c25505f33ff72e88b2990887c9dcf230f23259da296eb814fc2b41af999
Status: Downloaded newer image for ubuntu:latest
b6f6ad544726e220c6610cc06cb30c8088b90aa664b5b666add939ea4ed691a3
```

But this time, in a bit different way. Here, we are going to run a container using ubuntu image and we are going to mount the volume "vol-ubuntu" on the container' "tmp" or temp directory. Again, we will not do anything with this volume, since this demo primarily focuses on creation of the volumes.

Now, let's list the volumes to see what we have created.

Let's type:
```bash
docker volume ls
```
```
DRIVER    VOLUME NAME
local     vol-busybox
local     vol-ubuntu
```

And as you can see, we have 4(2) volumes here.

Two of them are created by us, whereas two of them are created by docker using local volume driver. Just like every other object which we have created previously like images. networks or containers, we can also filter the output of the ls command.

Let's type `docker volume ls` and put the filter of `dangling=true`, it means that it will **list the volumes which are not being mounted to any container**. Here, **vol-busybox** has not been mounted to any container.

```bash
docker volume ls --filter "dangling=true"
```
```
DRIVER    VOLUME NAME
local     vol-busybox
```

Similarly, the one above it which is provisioned by Docker is not being mounted or used currently.

Also, we can inspect our volume just like every other object by using docker volume inspect followed by the volume name.

```bash
docker volume inspect vol-ubuntu
```
```json
[
    {
        "CreatedAt": "2024-02-06T15:47:53+01:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/snap/docker/common/var-lib-docker/volumes/vol-ubuntu/_data",
        "Name": "vol-ubuntu",
        "Options": null,
        "Scope": "local"
    }
]
```

And as you can see, we get the creation timestamp, driver type, labels (which are null here), mount point, name of the volume, and scope (which is local).

Now, let's start to remove one of the volumes which we have created.

Type the command `docker volume rm` followed by the **volume name**.

Here we are using volume vol-ubuntu.

```bash
docker volume rm vol-ubuntu
```
```
Error response from daemon: remove vol-ubuntu: volume is in use - [b6f6ad544726e220c6610cc06cb30c8088b90aa664b5b666add939ea4ed691a3]
```

As you can see, we get an **error response** from docker daemon, it says that this volume cannot be removed **because** it is **in use** which means it has been mount to a container, so if we remove the volume, the container and its performance will be affected.

Let's get a list of containers to see which container is blocking our action of removing the volume.

```bash
docker ps -a
```
```
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS                      PORTS                                     NAMES
b6f6ad544726   ubuntu           "/bin/bash"              9 minutes ago   Exited (0) 9 minutes ago                                              busy_hodgkin
095e28c7829a   img_expose       "nginx -g 'daemon of…"   25 hours ago    Exited (255) 24 hours ago   0.0.0.0:32768->80/tcp, :::32768->80/tcp   cont_nginx-A
bf877f3ab875   img_expose       "nginx -g 'daemon of…"   25 hours ago    Exited (255) 24 hours ago   0.0.0.0:8080->80/tcp, :::8080->80/tcp     cont_nginx
0684e722b0ab   busybox:latest   "sh"                     26 hours ago    Exited (255) 24 hours ago                                             cc-busybox-A
184d803c366a   img_run-env      "bash"                   4 days ago      Exited (255) 4 days ago                                               cont_run-env
1c9d3fc97857   nginx:latest     "/docker-entrypoint.…"   6 days ago      Exited (0) 6 days ago                                                 web-server-nginx
44c5e486a49d   hello-world      "/hello"                 6 days ago      Exited (0) 6 days ago                                                 clever_solomon
```

And as you can see, the tender_noyce (busy_hodgkin) container which is built from the ubuntu image just two minutes ago has been mounted with the volume **vol-ubuntu**. Although, it has not been mentioned here.

You can guess it since all the other containers are up for more than an hour ago.

Let's type the command `docker container rm` followed by its name. And tender_noyce (busy_hodgkin) is removed!

```bash
docker container rm busy_hodgkin
```
```
busy_hodgkin
```

Now, let's re-run the command `docker volume rm vol-ubuntu`. This time, we didn't see any error and the volume should have been removed.

Let's verify it by listing the volumes again.

```bash
docker volume ls
```
```
DRIVER    VOLUME NAME
local     vol-busybox
```

And yes! the **vol-ubuntu** is not visible.