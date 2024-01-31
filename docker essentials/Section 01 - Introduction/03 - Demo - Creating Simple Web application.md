```bash
sudo su -
wget https://nginx.org/keys/nginx_signing.key
cd /etc/apt
ls
nano sources.list
```

paste :
```
deb https://nginx.org/packages/mainline/ubuntu/ jammy nginx
deb-src https://nginx.org/packages/mainline/ubuntu/ jammy nginx
```

```bash
apt-get remove nginx-common
apt-get update
apt-get install nginx
```

```bash
sudo /etc/init.d/apache2 stop
```

> [http://localhost:80](http://localhost:80)



## Transcription

Let's install nginx web server on our local machine. Nginx web server is the most vanilla example

of a web application. For your information, we are running Ubuntu 16.04 on this machine.

And now let's start with switching to the root user privileges. As you can see we moved to root privileges.

Now we can start our installation by first of all downloading the PGP or pretty good privacy key for Nginx.

The purpose of doing so is to make sure that when we install Nginx, the binaries are verified. The

key has been downloaded.

Now let's switch to etc/app directory. With ls

command let's list all the contents.

We have a bunch of files here.

But what we need is sources dot list file.

So let's open sources dot list with nano text editor.

You can use any text editor you like but in this course, we will mostly stick to nano. As you can see,

this file contains a lot of links. These links are sources for Ubuntu to find updates.

At the end of the file paste these two lines. These lines indicate the update path for Nginx application

when it gets installed and we update it further in the future. Let's save the file and exit Nano. Just

to make sure we don't have any dangling Nginx installation run apt-get remove nginx-common. This

command will make sure that any of the previously installed instance of Nginx installation is completely

removed.

Now let's run apt-get update to reflect the changes we have made in sources dot list file.

Let's use cd command twice to go back where we started.

Now let's install Nginx using apt-get install Nginx command.

Once the installation is complete, we can verify it by going to a web browser and opening localhost on

port 80.

The installation was successful!

Nginx is running properly.

This was an example of installing and running the most simple and vanilla web application Nginx

web server.