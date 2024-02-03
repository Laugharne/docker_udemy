
```bash
cd CC_Docker/S2/D7
```

Let's do something everyone in IT is quite hyped about it.

That's right.

We will containerize an application with Dockerfile. Although we have been doing it in previous demos as well,

In this demo, we really try to cover most of the instructions of Dockerfile and achieve something with them.

We have navigated to D-7 in S2 directory and it also has a Dockerfile.

Let's open it.

```Dockerfile

FROM ubuntu:12.04

LABEL Creator: "Cerulean Canvas"

RUN apt-get update && apt-get install -y apache2 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2

EXPOSE 80

CMD ["/usr/sbin/apache2", "-D", "FOREGROUND"]
```
> **12.04** --> **16.04** (*12.04 is too old*)

Starting from the beginning we have declared **Ubuntu 12.04**, for a change, as a base OS using `FROM` instruction.

We have also added metadata in form of a `LABEL` and mentioned the name of the creator of this file which is **Cerulean Canvas**.

Going further, `RUN` instruction will perform a general update, install Apache2 (which as you may very well be aware of is a web server), and clean up afterwards.

As you may have already guessed by now, three entries of ENV which means environmental variables are setting up environment of USER, GROUP, and LOG directory for our apache run-time. Here, www-data is the default user for web service running on Ubuntu and mentioning it makes the access of permissions for Apache to smoother.

We are exposing our container's port 80 using EXPOSE instruction.

And finally, we are executing the Apache2 web-server from its executable path.

We have used the CMD instruction for it and Apache will run as a foreground application, not as a daemon process.

Let's exit the file.

```bash
sudo docker build -t img_apache - < Dockerfile.txt
```

Now let's build our image with `img_apache` tag. By now, i'm sure you have seen the image building process enough times to comprehend each step that you see on screen.

Our docker image has been built successfully. As you can see, it is on top of the image list.

If you are wondering why we have still kept all of the images from previous demos, there is a reason and we will get to that pretty soon.

In the course. Now, let's run that image as container with `cont_apache` tag.

```bash
sudo docker run -itd --rm --name cont_apache -p 8080:80 img_apache
```


As you can see we have mapped our host's port 8080 to container's port 80 to view results on localhost.

Our container is also up and running.

Now let's open a web browser.

I have used Google Chrome in incognito mode just as it is my usual preference, but you can use whichever browser you like.

Let's open localhost and see what it is serving.

All right! localhost

is serving Apache's default page which means all of our setup worked correctly and we have successfully

containerized our web server. In next segment, we will play with different docker images and containers.