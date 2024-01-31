
```bash
sudo apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88 # <-- last 8 characters  of the key ID (shown above)

# You should see something like:
# pub   rsa4096 SHA256:cAmaD8B3RNHfGhVQX7sK1YaLMTIzCdqWwUOEeJbsvGk=
#          Key fingerprint = 9DBFCPOGE2B1AO2HSZPLPMW3REG5ZTDO55CO5Q
# uid           [ unknown] Docker Release (Debian) <docker@docker.com>
# sub    rsa4096 SHA256:1234567890ABCDEF

sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu/ $(lsb_release -cs) stable"
sudo apt update

sudo install docker-ce # `ce` for `Community Edition`, which is what we're using today

sudo docker run hello-world #  this will pull and run a test container, then remove it when done
```

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec31eb5944: Pull complete 
Digest: sha256:4bd78111b6914a99dbc560e6a20eab57ff6655aea4a80c50b0c5491968cbc2e6
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```

```bash
sudo groupadd docker
# add your user to the docker group so you don't need to use sudo to run docker commands
sudo usermod -aG docker ${USER}

```

## Transcription

In this demo. we install Docker on Ubuntu 16.04 orUbuntu Xenial.

Let's start off with running a standard apt-get update

command.

Once we are done with that, let's install some of the prerequisites, such as app-transport-

https to make sure that our machine can communicate through HTTPS, authority certificates, curl, and software

properties common which contains some of the Go lang objects which will be used by Docker. And the installation

is successful!

Now, let's download GPG key for Docker and add it to our machine. And to make sure that we don't get a long

list of processes which happen in the background.

Let's use -fsSL flag to keep our result as small as OK and it shows

OK!

Which means we got our GPG key.

Let's verify this key using sudo apt-key fingerprint

command. We can verify that we have received the correct key by searching for the last eight characters

of the fingerprint which should be 0

EBF CD88.

This information is provided by Docker itself,

so there is not much for you to figure out.

and Yes! Our key does have those characters as its last eight digits.

Now, run this command to add a repository called "stable" and add the content of download.docker.com

/linux/ubuntu on it.

We have provided the flag "(lsb_release -cs)" to make sure that Docker provides correct

files which means files for Ubuntu Xenial or Ubuntu 16.04 to our stable repository.

Let's run the the update again to reflect the changes.

Run " sudo apt-get install docker-ce " to finally install Docker. " -ce " stands for Community Edition which is

one of the two additions provided by Docker.

The other one is called Enterprise Edition, which is not free so we won't be including it in this course.

The process has ended and we have successfully installed "docker-ce" or Docker Community Edition. Verify that

our installation is successful by running "sudo docker run hello-world"

command. This will run a container called "hello-world" which would only be possible if the Docker installation

was successful.

You don't have to pay much attention to the processes which are going on because we will be exploring

all of them in sufficient depth in further modules.

As it says, our installation appears to be working correctly.

You may have noticed that we have been using root privileges over and over. To make sure that you can run

Docker from your regular user as well,

Let's perform a few more steps.

First, let's add a group called Docker using "sudo groupadd docker"

Now let's add our user which is dhwani to this docker group and provide it root privileges.

Now, let's try to run hello-world container without root privileges with just "docker run hello-world" command.

and we get the same results!