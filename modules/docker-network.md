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


TODO: overview of commands

```
  disconnect               Disconnect container from a network
  inspect                  Display detailed network information
  ls                       List all networks
  rm                       Remove a network
  create                   Create a network
  connect                  Connect container to a network
```

TODO: ls, go over
bridge
none
host

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

