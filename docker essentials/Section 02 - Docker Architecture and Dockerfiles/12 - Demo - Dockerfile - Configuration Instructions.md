

Let's write our first Dockerfile and understand its fundamental instructions.

Let's see what is our current working directory.

```bash
pwd
```


We are in the dhwani directory which is the user's name under home directory.

It is quite likely that you would also be in a similar location. Once you have downloaded the material provided in codes and lecture notes and unzipped it, you should also have a directory called "CC_Docker" where "CC" and "D" are capital.

```bash
$ tree CC_Docker/
CC_Docker/
├── S2
│   ├── D1
│   │   └── Dockerfile.txt
│   ├── D2
│   │   └── Dockerfile.txt
│   ├── D6
│   │   └── Dockerfile.txt
│   └── D7
│       └── Dockerfile.txt
└── S6
    └── docker-compose.yaml

6 directories, 5 files
```

We are only looking one level deep tree in our present directory, and if tree is not available on your machine for some reason you can verify the `CC_Docker` directory simply using ls command.

Now, let's navigate to the `CC_Docker` directory just to get you familiar with the structure of the directory, you will find one directory for each segment or module and subdirectories for respective demos.

If you don't intend to write the files by yourself while learning, you can simply use the appropriate files for each demo and run the results.

Let's go further to `S2` directory which contains all of the required codes and files for this segment.

We are in the `S2` at the moment.

Finally, let's navigate to the directory named `D1` and verify that we are at the right place. 

Further, Let's create an empty Dockerfile with touch command. I'm creating this file because I want to show you step by step how to write a Dockerfile, but you will find a pre-written Dockerfile in the directory. we are using "nano" as a text editor.

But again you're free to choose the one you might be comfortable with.

And with this let's open the empty Dockrefile and start writing it.

```dockerfile

ARG CODE_VERSION=16.04

FROM ubuntu:${CODE_VERSION}

RUN apt-get update -y

CMD ["bash"]
```


The first instruction that we are providing is the `ARG` or **"ARG" instruction**. `ARG` is used to define the arguments used by FROM instruction. Although, it is not necessary to use `ARG` and not using so does not cause any harm to the resulting image directly.

Sometimes it helps skipping parameters such as versions under control.

Here, we have defined argument **"CODE_VERSION=16.04"**, which means that we are going to use something which will have the code version 16.04 in a very rough sense. Remember, in a very rough sense, you can treat it as a declarable directive in general programming such as macros.

But again this argument will only be relevant for the FROM** instruction**.

And next is the `FROM` instruction. `FROM` is used to specify the base image for the resultant docker image that we intend to create.

**In any case**, a `FROM` instruction must be there in any Dockerfile and the only instruction that can be written before it is the ARG which we just saw.

Generally, `FROM` is followed by an **operating system** image or an **application** image which is publicly available on Docker Hub. Here, we want to have **Ubuntu** as our base operating system image with code version or **OS version 16.04**.

So, the name of the image is followed by a colon, an argument is mentioned in curly braces preceded by a **dollar sign**.

As we have already mentioned in ARG instruction. our code version is **16.04**.

So, it will be passed as an argument and the base image for this Dockerfile will be considered as **Ubuntu 16.04**.

To add a little more substance to the image, We are also including a set of `RUN` and `CMD` instructions, but we will explore their meanings and applications in next demos.

```Dockerfile
RUN apt-get update -y

CMD ["bash"]
```

For now, let's just save this file.

Again, it is important to remember that we must not give any extension to the Dockerfile, and should mostly name it as Dockerfile itself.

It is time to build the Dockerfile and turn it into an image.

Let's do it with `docker build` command.

```bash
sudo docker build -t img_from .
```

The `-t` option is used to tag the image or in other words, name the image to make it easily recognizable.

We will tag the image as `img_from` and the **dot** (`.`) in the end, directs Docker to the Dockerfile stored in the **present directory**.

As you can see the image is being built up step by step.

Let's understand each of these steps.

1. **First step** of storing argument was fairly simple so it finished quickly.

2. **Second step** involves setting up the base image and it is doing so by pulling multiple file system layers from Docker Hub and stacking them in proper hierarchy.

3. Once it is complete, it moves to the **third step** which is to a the update of the OS and we have already provided the permission with `-y` flag where y stands for yes.

Once these steps are done our image is built.

We can verify that the image is built via `docker images` command.

As you can see, we have four docker images among which **`img_from`** is the one which we created recently means 11 seconds ago.

While others are previously created or pulled.