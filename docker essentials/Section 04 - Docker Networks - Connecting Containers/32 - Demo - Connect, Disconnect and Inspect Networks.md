

In this demo, we will connect one of our containers with one of the networks that we have created.

First of all, let's see if we have any running containers.

```bash
docker ps -a
```

The container should be in running state since network object connectivity in docker follows the rules of interprocess communication in Linux.

Which means if there is no process, nothing can talk to it in terms of networks.

As we can see, we have two of our spare containers from previous module, but both of them are in exit state.

Let's start `my-ubuntu` container.

```bash
docker start my-ubuntu
```
Now, keep a list of networks in front of us to make better decisions.

We will use `docker network connect` command followed by network name and container name and hit enter.

```bash
docker network connect my-bridge-1 my-ubuntu
```

We don't get any sort of response like network ID or container ID from docker.

So a fair way to verify the connection would be to use `docker inspect` command. After using inspect on `my-ubuntu`,

```bash
docker container inspect my-ubuntu
```

If you navigated to the networking fields of the output, you can see that we have a description of bridge network

**my-bridge-1** attached to **my-ubuntu** container and it also has the **alias** which is same as the one we had received after creation of that bridge network.

You can also notice the endpoint which is described with an endpoint ID. In our

next command, instead of using a separate command to connect the docker network, we will mention it along with the `run` command using network flag.

```bash
docker container run -itd --network host --name cont_nginx nginx:latest
```

Here we are providing host network to the container named "cont_nginx" which will be created from nginx image having the latest tag.

Notably, if you run `docker container port` command with `cont_nginx`, you won't receive the port mapping information since no port mapping takes place with host network driver.

```bash
docker container port cont_nginx
```

Container communicates to Internet using port of host itself! We can view more information about this host network using inspect command on container.

```bash
docker container inspect cont_nginx
```

And as you can see, we can get network ID and endpoint details of the host network instance. Just like in previous container,

you can notice a field named "bridge" under network settings. This field is empty.

The reason is if we do not provide any network manually, docker provides the default bridge network to every container.

Now, let's inspect the default bridge network.


```bash
docker network inspect bridge
```

It seems that it too has its endpoint, subnet, and IP address range.

Now, if we look at the container's field, we will find `my-ubuntu` or to be precise, only `my-ubuntu`.

The reason why `cont_nginx` is not listed here is that it is connected to the host network.

Docker connects the container to one of the default networks and mostly the priority is bridge unless

we mention otherwise explicitly. Note the IP address of `my-ubuntu` under default bridge network which is "172.17.0.2"

Now, let's inspect user-defined bridge network.

```bash
docker network inspect my-bridge-1
```

In our case, `my-bridge-1` network. It has similar parameters compared to default bridge apart from different endpoint, IP range, and IDs.

It also has `my-ubuntu` container connected to it but the IP is different from the default bridge.

In other words, `my-ubuntu` container can be accessed from both the networks using corresponding IPs.

We can also format the output of inspect command like we used to do it previously.

```bash
docker network inspect --fomat "{{.Scope}}" bridge
```

Let's grab the value of scope field of default bridge network or we can grab a set of ID and name for the same. As it is visible in the output.

```bash
docker network inspect --fomat "{{.ID}} {{.Name}}" bridge
```

The first entry is the network ID and the second one followed by a colon is the network name.

List our containers again to see what to do next.

Well, we can see what happens when we disconnect the network from a container. Let's user `docker network disconnect` command followed by network name and container name which are `my-bridge-1` and `my-ubuntu` in this case.

```bash
docker network disconnect my-bridge-1 my-ubuntu
```

Finally, if we inspect our network, we can see that container `my-ubuntu` which was previously mentioned there is successfully out of sight.

```bash
docker container inspect my-ubuntu  
```

Similarly, if we inspect the container, we won't find the user-defined network either.