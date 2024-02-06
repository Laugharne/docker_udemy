In this demo, we will create our first docker network and understand it.

```bash
docker network create --driver bridge my-bridge
```

We will do it by using `docker network create` command and furnish it with driver flag. Our driver for this demo is a **bridge network**, so we will pass the argument bridge and finally, we will give it a suitable name `my-bridge`.

What we get as a result is an **ID** for the network object which has been created.

```
461e3e8b0f78e4508368fa45babb9a702739a671b1cb1a5a0c85a93880f8e897
```

Now, before we dig deep into my bridge, let's create another network called `my-bridge-1`.

```bash
docker network create --driver bridge --subnet=192.168.0.0/16 --ip-range=192.168.5.0/24 my-bridge-1
```

We will provide a few more parameters with this one for a better comparison.

Apart from the previously provided flag `driver` and its value `bridge`, we have also provided the subnet and IP range.

Again, we received **another ID**.

```
01300d72e3ef36904897677a2d622be3db802678cba139e839fda7e826f61f5e
```

Let's list these networks out.

```bash
docker network ls
```

As you can see, `my-bridge` and `my-bridge-1` are **not the only** available networks on the list.

```
NETWORK ID     NAME          DRIVER    SCOPE
b1228a168eb1   bridge        bridge    local
a22f9e762ad8   host          host      local
461e3e8b0f78   my-bridge     bridge    local
01300d72e3ef   my-bridge-1   bridge    local
28f2951fea26   none          null      local
```

That is because docker provides us a set of default created networks using different network drivers.

Here, they are **bridge**, **host** and **none**. You can tell by the names that bridge and host are using corresponding network drivers.

None is a special case though, it is used to indicate pure isolation and lack of connectivity.

We can also filter the search by providing the `filter` tag.

Let's put the **filter** that we only desire **bridge** network, so that driver field will be set to bridge.

```bash
docker network ls --filter driver=bridge
```

And here we have all the networks are created with bridge network driver.

```
NETWORK ID     NAME          DRIVER    SCOPE
b1228a168eb1   bridge        bridge    local
461e3e8b0f78   my-bridge     bridge    local
01300d72e3ef   my-bridge-1   bridge    local
```
