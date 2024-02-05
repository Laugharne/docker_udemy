

Let's start our demo where are we have ended the previous one.

The list of containers is still the same.

```bash
docker ps -a
```

Just the time duration has updated. In previous demo, we had created the container called `cc-busybox-A`, but we did not run it.

Now, to send it into running state, let's use `docker container start` command followed by the name of the container.

```bash
docker container start cc-busybox-A
```

We don't have to provide flags like `-itd` since they have **already been passed during the create command**.

Let's run it. We won't even get a container ID here since that too had been generated previously.

All we will get is the name of the container as a nod to success of the command

In typical docker CLI style. Time to get reperative and list out the containers again using `docker ps -a`.

```
0684e722b0ab   busybox:latest   "sh"                     25 minutes ago   Up 5 seconds                        cc-busybox-A
```

And we have an update!

Our created container `cc-busybox-A` is now finally in the running state!

Just like `start`, we also have a command to `stop` the containers since A has just started running.

Let's stop cc-busybox-B. Again, a confirmation signal is the name of the container.

```bash
docker container stop cc-busybox_B
```
And if you want to verify it, let's list all containers again and wait!

Where is our `cc-busybox_B`? Does that mean there is an error? Well, No.

If you remember, we had applied a **flag** called `rm` in our last demo with docker run command on `cc-busybox_B` container, which meant that the container will be **deleted** once it has **stopped** running.

- The use is simple, if you wanted to re-use the container, keep it.
- If you don't want to use it, remove it and free up some resources.

Next, we have the `restart` command, Let's restart our `cc-busybox-A` container. We will also give it a buffer of 5 seconds, and when we verify it what we get is a fresh restarted container up and running!

```bash
docker container restart --time 5 cc-busybox-A
```

Finally, i think all of us would agree that cc-busybox-A was not that great of a naming convention to follow.

It's just lengthy, over-complicated and bland!

If you encounter such thoughts with your container, we have a command to rename them.

Let's be a bit more casual and rename `cc-busybox-A` as `my-busybox`.

```bash
docker container rename cc-busybox-A my-busybox
```

And when we list them, we can see the changes reflected! By the way, notice that the container has just been renamed not restarted, which means we can rename them almost whenever we want unless they affect some other containers.