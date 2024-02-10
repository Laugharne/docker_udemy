

In this demo, we will execute the docker compose file which we created in the previous demo.

```bash
cd CC_Docker/S6/
```

Now, if you are in the present working directory and if your directory consists of only one **"docker-compose.yaml"** file, All you need to write is `docker-compose up` command followed by `-d` tag.

Of course, `-d` tag is optional and the only command which we are providing is `docker-compose up`.

```bash
docker-compose up -d
```
```

```


As you can see, it is creating objects one by one and if you notice even though we didn't provide any network information in our previous demo, first of all it is creating a "default network" with the default network driver.

It will be a bridge network. Then, it is creating the volumes "wordpress_files" and "db_data" from default driver.

So their scoops will be local.

Then it is creating services.

If you notice, "db" service is created before wordpress service because wordpress is dependent on "db".

Now, let's have a list of running containers to see if our service has created both the containers. And as you can see, "mysql_database" and "wd_frontend" both of the containers are up and running for more than 30 seconds.

Now, if you see further, in case of "wd_frontend" container even the port mapping information

is available where 8000 port is mapped to the 80 port. You may wonder, how did this happen?

We did not provide any information regarding any network.

If you remember, when we used docker-compose up command, docker compose first of all created a default network.

This network was created to make sure all of the network requirements of the preceding services will

be fulfilled by it, in terms of bridge network. Which means that both of these containers are connected to the same default bridge network so they can talk to the outside world and they can talk to each other.

Now, let's go to our web browser and see what is being hosted on our localhost. As we can see, the localhost is hosting the default page of Wordpress installation which means that WordPress installation and hosting was successful!

Now, let's play a bit more with this Wordpress and see what we can do with it.

WordPress blog: Work in Progress!

Let's Get Started!

Phewww.. We have added a lot of content to a dummy post and now it says that the Post has been published.

If we click on View Post button, we should be able to view how our post looks.

So let's do that.

The post looks neat, tidy, and well structured.

It means that WordPress installation was not only successful it is working just smoothly!

Now, let's work with mysql.

This may not look as exciting and vivid as Wordpress web page, but we are back to our good old terminal.

Now, let's again get a list of running containers. We have already worked with wwd_frontend, so now, it's time to work with mysql_database container.

Let's run it with "docker exec -it" command and run a "bash" command on it.

We are into the container with root privileges, so let's list all the directories.

Let's navigate to "/var/lib/mysql" directory to see it's content further. As you can see, the information about WordPress user has already been added to this container, which means that the linking of these containers was successful and the information was exchanged successfully as well!

Let's run another instance of mysql container.

But this time as a client. As you can see, we are linking this container with our previous

mysql_database container and we are also providing information about the communication port and root user credentials.

You may have to connect it to default bridge network. As you can see, the client side of mysql is now active and we can see what is being hosted on mysql_databse server.

When we run the query "show databases", apart from the system provided or default databases like information schema, mysql, performance schema, or sys itself!

We also have the 5th database called "wordpress" which has been derived from the service of "wordpress front-end"

If we go further into this, let's use the query "use wordpress" so that we can dig deeper into that database.

Now, our database has changed.

Let's take a look at the tables inside wordpress database. Type "show tables;" and hit enter.

And here we are!

All the required tables for a successful wordpress instance.

Although, we don't need to doubt whether this was working properly or not because WordPress was already established and it was working so smoothly.

But this gives us even stronger belief and understanding on how linked services work with Docker Compose.