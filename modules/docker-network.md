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


## Create a Network

TODO: create a network
docker network create live
docker network create test

docker network ls
docker network inspect test
bridge

TODO: create a container
shutit.send('docker run -d --name container1 debian sleep infinity',note='Start a container')
go into it (test)
		shutit.login(command='docker exec -ti container1 bash',note='Observe that mysmallsubnet is now connected to this container, and it has been allocated an IP address in the subnet we specified.')
TODO: install some tools
		shutit.send('apt-get update && apt-get install net-tools')
		
		shutit.send('docker inspect container1',note='''Inspect this container's network section. Observe it has only one IP address''')

		shutit.send('ifconfig',note='Observer that this container now has another eth interface with the relevant address')

TODO: connect it to test network
		shutit.send('docker network connect mysmallsubnet container1',note='Connect the container just-created ')
		shutit.send('docker inspect container1',note='See how mysmallsubnet is now connected to this container, and it has been allocated an IP address in the subnet we specified.')

TODO: some pings

TODO: disconnect
		shutit.send('docker network disconnect mysmallsubnet container1',note='Connect the container just-created ')
		shutit.send('docker inspect container1',note='See how mysmallsubnet is now connected to this container, and it has been allocated an IP address in the subnet we specified.')

TODO: connect to live


<!-- @end -->

