# Boot2docker
This is an alternative to boot2docker to  make it work with vmware fusion until official support for fusion shared folders lands in boot2docker or docker/machine. All boot2docker images we have tried so far don't work properly with HGFS which is the most effective way to share folders with vmware fusion.
# VMware fusion
This image requires license for [vmware fusion provider for vagrant](http://www.vagrantup.com/vmware) to create and provision the machine and make sync folders work.
Creation of the machine can be automated using packer but sync folders is quite a complicated problem to solve without vagrant.

# Usage
To start initial provisioning use:
`vagrant up`
Then you need to point local docker to this docker host. Vagrant will alias vm IP to `docker.local` automatically.

Then just do

`export DOCKER_HOST=tcp://docker.local:2375`

## If it doesn't work
Get machine ip from
`ping docker.local`

Export it

```export DOCKER_HOST=tcp://IP_OF_VM:2375```

## Check
To check that all is working do
```
> docker version
Client version: 1.5.0
Client API version: 1.17
Go version (client): go1.4.1
Git commit (client): a8a31ef
OS/Arch (client): darwin/amd64
Server version: 1.5.0
Server API version: 1.17
Go version (server): go1.4.1
Git commit (server): a8a31ef
```

If you get different version of docker between client and server update/downgrade your local version. When you provision the machine it will install the latest docker.

## Shared folders (aka Magic folder)
To make shared folders mounting transparent from the host we mount the whole home directory under /Users/<USER_NAME>. That way you can just mount folders under home directory without worrying about path difference between host and virtual machine.

For example,
`docker run -v /Users/john/src/play2lead:/myapp`
mounts `/Users/john/src/play2lead` directory from VMware machine to docker container. The directory on the VMware machine syncs with host automatically.
