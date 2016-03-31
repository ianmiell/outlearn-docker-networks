<!--
{
"name": "docker-networks",
"version" : "0.1",
"title" : "Docker Networks",
"description" : "Get acqainted with Docker networks",
"homepage" : "https://github.com/ianmiell/outlearn-docker-networks",
"freshnessDate" : 2016-03-28,
"license" : "CC BY 4.0"
}
-->
<!-- @section -->

# Docker Install

In this first part we're going to check you have the right version of Docker.

At your terminal, type:

```
docker network
```

and you should get some output that looks like this:

```
docker: "network" requires a minimum of 1 argument.
See '/usr/bin/docker network --help'.

Usage:	docker network [OPTIONS] COMMAND [OPTIONS]

Commands:
  connect                  Connect container to a network
  disconnect               Disconnect container from a network
  inspect                  Display detailed network information
  ls                       List all networks
  rm                       Remove a network
  create                   Create a network

Run 'docker network COMMAND --help' for more information on a command.
```

If your version is version 1.8 or lower (run 'docker version' to find out), then you may find that you see output that looks like this:

```
docker: 'network' is not a docker command
```

and will need to install the latest version of Docker (see below)

If you see output that looks like this:

```
docker: command not found
```

Then you need to install docker first.


See [here](https://docs.docker.com/engine/installation/) for how to install the latest version of Docker.

<!-- @section -->

# Docker Network

If you type 

```
docker network
```

you will see the following output (or similar):

```
docker: "network" requires a minimum of 1 argument.
See '/usr/bin/docker network --help'.

Usage:	docker network [OPTIONS] COMMAND [OPTIONS]

Commands:
  connect                  Connect container to a network
  disconnect               Disconnect container from a network
  inspect                  Display detailed network information
  ls                       List all networks
  rm                       Remove a network
  create                   Create a network
  connect                  Connect container to a network
```

The commands listed are all the actions that can be performed on networks managed by Docker. Yes, you can create, inspect and rm (remove) networks just like you can with containers!

## List Networks

First, list the networks that already exist by running:

```
docker network ls
```

and you should see three entries:

```
NETWORK ID          NAME                DRIVER
4fefe37175f7        bridge              bridge              
dd3db2224b56        none                null                
d42661b5938d        host                host         
```

Each line corresponds to a network. The ID is arbitrary, so that you can reference it.

The name is a reference name for the network, and the driver indicates the type of network as Docker sees it.

The 'bridge' driver (and default 'bridge' network) are the default networks you are used to from . It's called a 'bridge' because it connects a virtual ethernet interface to the default ethernet interface on your host machine, allowing you to connect from your container to the wider internet.

The 'none' driver simply means 'no network', and the 'host' driver means 'use the network of the host machine directly'.

Other network drivers include 'overlay' (for multi-host networking) and 'weave' (a popular third party plugin).

## Inspect a Network

If you run an inspect command on the network:

```
docker inspect bridge
```

then the output will look similar to this:

```
[
    {
        "Name": "bridge",
        "Id": "4fefe37175f72c33e0676685b62c1bebf1099140a5569b7a22f66484aeb4260f",
        "Scope": "local",
        "Driver": "bridge",
        "IPAM": {
            "Driver": "default",
            "Config": [
                {
                    "Subnet": "172.17.0.0/16"
                }
            ]
        },
        "Containers": {},
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        }
    }
]
```

This is some json output that represents the network setup for this Docker network object. Note that the driver is 'default' (ie the traditional Docker bridging interface), and the bridge name is the possibly familiar 'docker0' you may have seen from the output of 'ifconfig'.

## Create a Network

Now create two networks of your own with:

```
$ docker network create live
bd134b5e5cf417f47ae75854276c6e28f35546c3d50580c3c38c7b4cded989b8
$ docker network create test
447137d802d40aeda4fa73287354079f47ad29c55ddcb2e86eaa52227d6e62f5
```

(Note that the IDs returned will be different for you).

Running docker ls will now show five networks:

```
$ docker network ls
NETWORK ID          NAME                DRIVER
447137d802d4        test                bridge              
4fefe37175f7        bridge              bridge              
dd3db2224b56        none                null                
d42661b5938d        host                host                
bd134b5e5cf4        live                bridge 
```

Now if you inspect the new ones you can see they are simple bridge networks with no options set:

```
$ docker network inspect test live
[
    {
        "Name": "test",
        "Id": "447137d802d40aeda4fa73287354079f47ad29c55ddcb2e86eaa52227d6e62f5",
        "Scope": "local",
        "Driver": "bridge",
        "IPAM": {
            "Driver": "default",
            "Config": [
                {}
            ]
        },
        "Containers": {},
        "Options": {}
    },
    {
        "Name": "live",
        "Id": "bd134b5e5cf417f47ae75854276c6e28f35546c3d50580c3c38c7b4cded989b8",
        "Scope": "local",
        "Driver": "bridge",
        "IPAM": {
            "Driver": "default",
            "Config": [
                {}
            ]
        },
        "Containers": {},
        "Options": {}
    }
]
```

You can see the interfaces that were created by using traditional network tools:

```
$ ifconfig br-bd134b5e5cf4
br-bd134b5e5cf4 Link encap:Ethernet  HWaddr 02:42:ae:18:d7:89  
          inet addr:172.18.0.1  Bcast:0.0.0.0  Mask:255.255.0.0
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
$ ifconfig br-447137d802d4
br-447137d802d4 Link encap:Ethernet  HWaddr 02:42:e1:be:f0:cf  
          inet addr:172.19.0.1  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:e1ff:febe:f0cf/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:16 errors:0 dropped:0 overruns:0 frame:0
          TX packets:39 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:1072 (1.0 KB)  TX bytes:5911 (5.9 KB)
```

Note that the interface name (br-bd134b5e5cf4) is 'br-' with the network ID seen above appended to it.

Each has been given its own subnet (172.18.0.0/16 and 172.19.0.0/16).

## Add a container to the test network

Now you have created these (sub)networks, you can . Just as you can create a container attached to 'net=host', or 'net=none', you can attach one to your newly-created test network. Here you run a busybox container that does nothing but sleep an hour.

```
$ docker run --name app1 -d --net=test busybox sleep 3600
c26f6a548c6d5754059f1c7654f77b63e239d3c58803805431b11c9bafbe4b05
```

If you run the 'ip addr' command you can see that the container has a network interface available on the 172.19.0.0/16 subnet...

```
$ docker exec app1 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
594: eth0@if595: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue 
    link/ether 02:42:ac:13:00:05 brd ff:ff:ff:ff:ff:ff
    inet 172.19.0.5/16 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:acff:fe13:5/64 scope link 
       valid_lft forever preferred_lft forever
```

...and that this corresponds to the output of docker inspect for that container. NetworkSettings.Networks.test.IPAddress is the same as the above interface (172.19.0.5).

```
$ docker inspect app1
[...]
    "NetworkSettings": {
[...]
        "Networks": {
            "test": {
                "EndpointID": "5e3a39196ffe0d576e7ef5eb89385236ec1fd4a509740ff2cbedcb59d7c54ea1",
                "Gateway": "172.19.0.1",
                "IPAddress": "172.19.0.5",
                "IPPrefixLen": 16,
                "IPv6Gateway": "",
                "GlobalIPv6Address": "",
                "GlobalIPv6PrefixLen": 0,
                "MacAddress": "02:42:ac:13:00:05"
            }
        }
```

If you test your access to the gateway using ping, we have access:


```
$ docker exec app1 ping -c 1 172.19.0.1
PING 172.19.0.1 (172.19.0.1): 56 data bytes
64 bytes from 172.19.0.1: seq=0 ttl=64 time=0.430 ms

--- 172.19.0.1 ping statistics ---
1 packets transmitted, 1 packets received, 0% packet loss
round-trip min/avg/max = 0.430/0.430/0.430 ms
```

## Disconnect running container from the 'test' network

```
$ docker network disconnect test app1
```

If we re-run the previous ping command, the network is reported as unreachable:

```
$ docker exec app1 ping -c 1 172.19.0.1
PING 172.19.0.1 (172.19.0.1): 56 data bytes
ping: sendto: Network is unreachable
```

## Connect running container to the 'live' network

Now that we've tested our container we are happy to move it to the 'live' network. We do this by running the 'docker network connect' command:

```
$ docker network connect live app1 
$
```

We have been given a new IP address (172.18.0.2). Again, the address and IDs will differ for you:

```
$ docker inspect app1
[...]
    "NetworkSettings": {
[...]
        "Networks": {
            "live": {
                "EndpointID": "09b9a7dbcde50b3e069984b6b1485880218b94919839d4523a63079edae5a221",
                "Gateway": "172.18.0.1",
                "IPAddress": "172.18.0.2",
                "IPPrefixLen": 16,
                "IPv6Gateway": "",
                "GlobalIPv6Address": "",
                "GlobalIPv6PrefixLen": 0,
                "MacAddress": "02:42:ac:12:00:02"
            }
        }
```

And we can ping the outside world again:

```
$ docker exec app1 ping google.com
PING google.com (216.58.213.174): 56 data bytes
64 bytes from 216.58.213.174: seq=0 ttl=61 time=7.244 ms
64 bytes from 216.58.213.174: seq=1 ttl=61 time=9.868 ms
```

<!-- @end -->

