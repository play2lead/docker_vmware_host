# Boot2docker
This is an alternative to boot2docker to  make it work with vmware fusion until official support for fusion shared folders lands in boot2docker or docker/machine. All boot2docker images we have tried so far don't work properly with HGFS which is the most effective way to share folders with vmware fusion.
# VMware fusion
This image requires license for [vmware fusion provider for vagrant](http://www.vagrantup.com/vmware) to create and provision the machine and make sync folders work.
Creation of the machine can be automated using packer but sync folders is quite a complicated problem to solve without vagrant. 
