

## Transcription

Containers are an abstraction at the application layer which packages codes and dependencies together.

Let's re-iterate and expand this definition further.

Containers are an abstraction at the application layer which packages codes and dependencies together.

It means instead of just shipping the applications, container ship the application and the run-time environment

as well.

and they still manage to remain small sized. How?

Let's compare them architecturally with VMs. In a traditional VM architecture,

we have a hypervisor like Hyper-V or KVM on top of hardware infrastructure.

These are also called Type 1 hypervisors.

Since they don't need a host operating system. The guest OSs are provisioned on top of hypervisor and they

acquired their isolated virtual environment. In some cases, we get Type 2 hypervisor like Oracle's

virtual box where we DO need a host operating system and the rest of the part plays out pretty much

the same.

And this is how VMs function in a very broad sense.

Coming back to containers, the biggest difference compared to VMs is they don't have guest operating

systems. Container runtime environment is used instead of hypervisor.

What is it you may ask? For now, let's said it is software which manages and runs containers. Containers

contain the application code and the dependencies as we have just seen. The dependencies don't only mean

external or third party libraries.

It also means OS-level dependencies.

The logic behind such implementation is all of the Linux variants share the same Linux kernel.

Well, more or less. So there is no point in duplicating the same set of files over and over in multiple

VMs.

If all containers can just access them in their own isolated in environment.

With that said, what about the files which are uncommon? or to be precise the files which are specific

to the OS? Well, containers will contain them along with the application and since the process of making

the containers and running them are done by the same container run-time environment, there would be no

conflict of environment. If this information is too sudden for you, don't worry.

The intention of mentioning all of these is just to let you know that how

containers can attain same level of isolation as VMs but while sharing the resources with host OS

instead of duplicating them and what happens because of that?

Well, containers consume less storage and memory. Without stretching the facts at all, gigabytes (GB)

literally turn into megabytes (MB). This way,

shipping them is easier as well.

We don't ship the whole VMs or a long list of instructions. We just ship ready to run containers!

And since all of the necessary dependencies are also packed with the containers,

if it worked on the developer's environment, it will work on your machine as well. Since we have reduced the resources,

scaling becomes easy and cheaper. Even though you need to create 10 more replicas of a back-end container,

you probably won't how to spend money on buying or renting a new server.

In fact, if you need to roll out updates you can still keep your application running by extending

your number of replicated containers and you may achieve zero downtime!

All of these sound attractive and groundbreaking, but if we did this to industries who are actually using

containers?

Well, Google pioneered using orchestrated containers years ago when they started facing overwhelming

amount of data. These days,

companies like "Expedia", "Pay-Pal", and "GlaxoSmithKline" are voluntary providing themselves as references

and case studies. Apart from them,

educational institutions like "Cornell University" and gaming giants like "Niantic", which became a huge success

after Pokemon GO are all using containers.

Companies are gradually migrating to containers. As many of you might already know, DevOps jobs are

increasing rapidly, and containers are an essential part of the whole DevOps movement.

In the next lecture, we will finally introduce ourselves with Docker and will get started with learning it.
