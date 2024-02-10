

As the title of this demo suggests, we are going to install **Docker Compose** in this demo.

We will do so by fetching the binary of Docker Compose from its official GitHub release and we will store this binary in `docker-compose` **directory** under `user/local/bin` on our host machine.

We will do it with a **curl** utility. Once the downloading is complete, we will make these binaries executable and the installation process will be complete!

```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
```

Let's see, if the installation is successful by running `docker-compose --version` command.

```bash
docker-compose --version
```
Docker Compose version v2.20.3
```

Well, the installation is successful and **Docker Compose version 2.20.3** is currently installed on our host.

This is the latest version, by the time this course is being created.