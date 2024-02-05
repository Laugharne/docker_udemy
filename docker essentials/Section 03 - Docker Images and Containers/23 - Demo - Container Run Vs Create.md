

Let's test out both of these commands with a busybox container.

```bash
docker container create -it --name cc-busybox-A busybox:latest
```

- First, we will use `docker container create` command.
- It is followed by `-it` tag, which means it will be **interactive** and **teletype** enabled.
- We haven't given it detached flag since we don't need to.
- We are naming our container `cc-busybox-A`, and we're using image `busybox` with `latest` tag.

When we run the command, since the content of the image is not available locally, it will be pulled from the **docker hub**. Once it is pulled, what you see at the end, is the unique container Id created by docker.

```
Unable to find image 'busybox:latest' locally
latest: Pulling from library/busybox
9ad63333ebc9: Pull complete 
Digest: sha256:6d9ac9237a84afe1516540f40a0fafdc86859b2141954b4d643af7066d598b74
Status: Downloaded newer image for busybox:latest
0684e722b0ab6072c769837bef02a15cdeeda7e7a5d3dc196811f54277223c2b

```

The **ID** (*1*) is **unique**, at least across the host and cluster, you are running any. Now our container should be created.

*(1) : 0684e722b0ab6072c769837bef02a15cdeeda7e7a5d3dc196811f54277223c2b*

To list out containers, we have to run the command `docker ps -a`.

```
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS                    PORTS     NAMES
0684e722b0ab   busybox:latest   "sh"                     3 minutes ago   Created                             cc-busybox-A
184d803c366a   img_run-env      "bash"                   3 days ago      Exited (255) 3 days ago             cont_run-env
1c9d3fc97857   nginx:latest     "/docker-entrypoint.…"   4 days ago      Exited (0) 4 days ago               web-server-nginx
44c5e486a49d   hello-world      "/hello"                 5 days ago      Exited (0) 5 days ago               clever_solomon
```

And once we do so, we get a list of all the containers which are running, are about to run, or hav finished running on this host. The output layout is fairly simple, and the topmost entry is our recently created container.

It is not in the running state yet, which can also be verified from the **STATUS** column.

It is followed by quite a few other containers, which have finished running and have exited some time ago.

Here, the resources are already ready to be allotted to the container, but haven't been allotted yet.

Don't worry! We will let this container enjoy its dream run as well.

But before that, let's see what happens when we run a container instead.

```bash
docker container run -itd --rm --name cc-busybox_B busybox:latest
```

You might find this command similar to what we have used in some of our initial demos.

It is because this is the most mainstream way to run it. This time, we also put the `d` flag so we don't have to dive into the container and we have named `cc-busybox_B`. Since we had already pulled the busybox image last time, docker has cached the entirety of it and has simply return us a container ID.

If you're wondering why do we have an `rm` flag tagging along, it instructs docker to **delete** this container **after** it has **finished running**.

Let's check out `docker ps -a` again, and what we see is our top entry replaced by **busybox_B** container.

```bash
docker ps -a
```
```
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS                    PORTS     NAMES
d5c05f95bedd   busybox:latest   "sh"                     5 seconds ago    Up 4 seconds                        cc-busybox_B
0684e722b0ab   busybox:latest   "sh"                     10 minutes ago   Created                             cc-busybox-A
184d803c366a   img_run-env      "bash"                   3 days ago       Exited (255) 3 days ago             cont_run-env
1c9d3fc97857   nginx:latest     "/docker-entrypoint.…"   4 days ago       Exited (0) 4 days ago               web-server-nginx
44c5e486a49d   hello-world      "/hello"                 5 days ago       Exited (0) 5 days ago               clever_solomon
```


Unlike its counterpart called **busybox-A**, this one is running for 6 seconds.

In fact, there is also a three second difference between its creation time and its running time.

You can assume that docker took that time to allocate the resources and register it as a process to its host.

Since we have our container running, we will play with it a bit more in the next lecture.