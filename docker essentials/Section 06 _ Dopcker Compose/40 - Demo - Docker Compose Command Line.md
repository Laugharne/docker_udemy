
Now, that we are done with docker-compose YAML file and its execution, Let's switch to docker-compose command line.

Our first command in the series of docker-compose commands is "docker-compose config".

```bash
docker-compose config
```
```

```

This command is used to **view the compose YAML file on the terminal screen**. As you can see, it provides all of the information about both the services and volumes, which we had mentioned in the previous YAML files.

We can also extract specific information from the YAML file like services. The next command is "docker-compose images". This command is used to list out all the images used to create containers for services and compose files. As you can see, both the images are available here which were used in the previous services of docker-compose.yaml file.

Our next command is "docker-compose logs". As you might have guessed, this command is used to fetch the log output from the service.

Since we help a lot of logs, let's narrow them down a bit using "docker-compose logs --tail=10", the "tail" flag allows the last 10 logs of both the services to be printed on the STDOUT or terminal. As you can see, we have last 10 logs of both services or containers, mysql and wordpress. Just like "docker ps", we have "docker-compose ps" where we can see both the containers running along with other information such as "state" which is up, "port mapping" information and "ENTRYPOINT" commands. Our next command is "docker-compose top" which is used to display all the running processes inside all of the containers.

Which means that in both the containers "mysql_datbase and wd_frontend", these are the processes which are running. Each processes have their individual process ID and parent process ID.

The structure of process and parent-child relationship depends on the base image used in the creation of these images.

And finally, we have "docker-compose down".

You can consider it as a cleanup command or a contrary command to "docker-compose up". When we hit enter, it stops both of the services, removes the containers and removes additional resources like networks.

In next module, we will have at probably the most exhaustive feature of Docker which is "Docker Swarm".