

Just like previous demos, we have a list of containers here.

```bash
docker ps -a
```

Now, let's use docker container attach command.

```bash
docker container attach cc-busybox-A
```

It means that we are attaching the **standard I/O** and **standard error** of our container to the terminal of our docker client.

We have attached to my-busybox container here.

So, let's hit enter.

As you can see, now we are accessing standard I/O or terminal of busybox from our Ubuntu terminal.

If we hit "ls", we will see a list of available directories in a busybox root environment. We can play around a bit more to navigate to other directories as well.
```bash
/ # 
/ # 
/ # 
/ # 
/ # ls
bin    dev    etc    home   lib    lib64  proc   root   sys    tmp    usr    var
/ # pwd
/
/ # whoami
root
/ # cd home/
/home # ls
/home # cd ..
/ # ps -l
PID   USER     TIME  COMMAND
    1 root      0:00 sh
   10 root      0:00 ps -l
/ # exit
```

If we `exit` it, we return back to our ubuntu host terminal. And there is an interesting aspect to the attach command.

When we list the containers again, we can see that my-busybox container is not running.
```bash
docker ps -a
```
```
CONTAINER ID   IMAGE            COMMAND                  CREATED          STATUS                          PORTS     NAMES
0684e722b0ab   busybox:latest   "sh"                     48 minutes ago   Exited (0) About a minute ago             cc-busybox-A
184d803c366a   img_run-env      "bash"                   3 days ago       Exited (255) 3 days ago                   cont_run-env
1c9d3fc97857   nginx:latest     "/docker-entrypoint.…"   5 days ago       Exited (0) 5 days ago                     web-server-nginx
44c5e486a49d   hello-world      "/hello"                 5 days ago       Exited (0) 5 days ago                     clever_solomon
```

It has **exited** a few seconds ago.

In other words, attaching a container conditions it to be stopped when we exit the attachment.

An alternative to this is `docker exec` command.

It allows us to use any command we want and it executes the container.

But before, let's start our container again.

```bash
docker container start cc-busybox-A
```

Now, we have used `docker exec` which stands for execute with `-it` flag and have directed it to run and print the result of `pwd` command.

```bash
docker exec -it cc-busybox-A pwd
```
```
/
```

Once it succeeds, we have a forward slash (**/**) which indicates **root** our busybox. Unlike attach, if we list the containers again, we will find our container is **still up and running**.