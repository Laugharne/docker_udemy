
In this demo, we will map our host machine's port to container's port. The command is fairly simple.

```bash
docker container run -itd --name cont_nginx -p 8080:80/tcp img_expose
```

As we just have to extend the `run` command with a flag. We will map our **host's port 8080** to **container's port 80** on **TCP** by mentioning it following `-p`. Notice that, the image we used here is the one that we had created while working with **EXPOSE** instruction.

Now when we run the containers, we get the ports mentioned in the output.

```bash
docker ps -a
```

The output looks a bit messy but the annotations should help here.

```
CONTAINER ID   IMAGE            COMMAND                  CREATED             STATUS                    PORTS                                   NAMES
bf877f3ab875   img_expose       "nginx -g 'daemon of…"   44 seconds ago      Up 42 seconds             0.0.0.0:8080->80/tcp, :::8080->80/tcp   cont_nginx
```

Now, we will create another container from the same image called `cont_nginx-A` Instead of providing ports and protocols like earlier, this time, we will just provide `-P` and allow docker to map ports by itself. 

```bash
docker container run -itd --name cont_nginx-A -P img_expose
```
Here, It will use the information provided by EXPOSE instruction in the Dockerfile and tally available ports from host machine's network drivers.

We can see that the new container has **port 80** mapped from container to port  **32768** of host.
```
CONTAINER ID   IMAGE            COMMAND                  CREATED             STATUS                    PORTS                                     NAMES
095e28c7829a   img_expose       "nginx -g 'daemon of…"   4 seconds ago       Up 3 seconds              0.0.0.0:32768->80/tcp, :::32768->80/tcp   cont_nginx-A
bf877f3ab875   img_expose       "nginx -g 'daemon of…"   3 minutes ago       Up 3 minutes              0.0.0.0:8080->80/tcp, :::8080->80/tcp     cont_nginx
```

We can also view this information by hitting `docker container port` command followed by the container name.

```bash
docker container port cont_nginx-A
```
```
80/tcp -> 0.0.0.0:32768
80/tcp -> [::]:32768
```

Finally, when we run [**localhost on port 8080**](http://localhost:8080) on our web browser, we can see nginx homepage which indicates that our port mapping was successful! When we do the same with the other container, it shows the same thing as well!