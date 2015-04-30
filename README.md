# Boot2docker
This is an alternative to boot2docker to  make it work with vmware fusion until official support for fusion shared folders lands in boot2docker or docker/machine. All boot2docker images we have tried so far don't work properly with HGFS/NFS which is the most effective way to share folders with vmware fusion.


You can use Virtual Box(ignore the name of the repo) or VMWare fusion to run this host.
# Virtual Box
Virtual box can only be used with NFS but it's still faster than running boot2docker with shared folders.

# VMware fusion
This image requires license for [vmware fusion provider for vagrant](http://www.vagrantup.com/vmware) to create and provision the machine and make sync folders work.
Creation of the machine can be automated using packer but sync folders is quite a complicated problem to solve without vagrant.
# Required software (install all of this before proceeding)
- [Docker client for mac](https://docs.docker.com/installation/mac/) >= 1.5.0
- [VMWare fusion](http://www.vmware.com/au/products/fusion) >= 7
- [Vagrant](https://www.vagrantup.com/downloads.html)
- [vmware fusion provider for vagrant](http://www.vagrantup.com/vmware)

# Usage
To start initial provisioning use:
`vagrant up`
Then you need to point local docker to this docker host. Vagrant will alias vm IP to `docker.dev` automatically, however it doesn't work all the time.

To check do
```
ping docker.dev
```
If alias worked you get the ip address.

Otherwise you need to get the ip via the following command
```
vagrant ssh-config | grep HostName | awk '{print $2}'
```
Export it for this session usage

```export DOCKER_HOST=tcp://IP_OF_VM:2375```

Or export it permanently
```
echo `vagrant ssh-config | grep HostName | awk '{print $2}'` docker.dev | sudo tee -a /etc/hosts
```
Then add to your ~/.bashrc/.zshrc
```
export DOCKER_HOST=tcp://docker.dev:2375
```

For **Google Chrome** users:

If you want fast and snappy chrome when using docker.dev alias you need to remove ipv6 alias from /etc/hosts otherwise chromes bugs out and becomes really slow.
In /etc/hosts replace
```
::1             localhost
```
with
```
#::1             localhost
```

## Check
To check that all is working do(version might change over time)
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

## Docker login
To use our private images you need to login to docker via your personal account which should be part of play2lead organisation.
Use `docker login` to do that before starting to build any repos.

## Notes
### Shared folders (aka Magic folder)
To make shared folders mounting transparent from the host we mount the whole home directory under /Users/<USER_NAME>. That way you can just mount folders under home directory without worrying about path difference between host and virtual machine.

The directory on the VMware machine syncs with host automatically.
For example,
`docker run -v /Users/john/src/play2lead:/myapp`
mounts `/Users/john/src/play2lead` directory from VMware machine to docker container.


# Thanks
To Graeme Mathieson for his [great article on setup vmware fusion + docker](https://woss.name/articles/vagrant-docker-and-vmware-fusion/)
