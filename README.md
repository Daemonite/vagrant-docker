# Vagrant machine for Docker

vagrant-docker is a [Vagrant](http://www.vagrantup.com/) definition for a virtual machine suitable for [Docker](http://www.docker.com) development.

## Main features

 * Supports the [Virtualbox](https://www.virtualbox.org/) and [VMWare](https://www.vmware.com/) VM providers.
 * Operating system optimised for running Docker server and based on [Ubuntu](http://www.ubuntu.com) so developers can install any of their favourite Linux tools easily.
 * Includes the most recent stable releases of Docker and [Docker Compose](https://docs.docker.com/compose/).
 * Default port mappings and shares for easy development and testing of Docker containers.
 * Includes the insecure SSH key for phusion's [docker-baseimage](https://github.com/phusion/baseimage-docker) so you can SSH into containers based on that image.
 
This machine uses Docker-friendly base boxes built by [Phusion](http://www.phusion.nl/). See their [base box git repo](https://github.com/phusion/open-vagrant-boxes) for details of the features of the base box.

## Configurable settings

The Vagrant machine has a small number of settings that can be configured by environment variables. Set these in your host user environment prior to running vagrant commands.

### DOCKERHOST_MEMSIZE

The amount of memory in MB to allocate to the VM. Default is 1024.

### DOCKERHOST_IPADDR

The IP address to assign to the VM on the virtual network that connects it to the host. By default Vagrant selects an unused IP address in a private network range.

### DOCKER\_FWDPORT\_MIN, DOCKER\_FWDPORT\_MAX

The Vagrant definition maps a range of high ports from the VM to the host. Docker containers that publish ports in this range will have those ports accessible from the VM host. By default this range is 48000-48199, but this can be changed with the DOCKER\_FWDPORT\_MIN and DOCKER\_FWDPORT\_MAX variables.

## Build folder

This VM enables the default share which maps the folder containing the Vagrantfile to the `/vagrant` folder in the guest VM.

If you create a folder named `build` under the project its contents will be ignored by Git. This makes it a suitable place to put your Docker Compose projects, Dockerfiles, and git repositories.


## Possible future enhancements

It may be useful to include a [Consul](https://www.consul.io/) server in standalone mode to support clustering via [Docker Swarm](https://docs.docker.com/swarm/) and service discovery through tools such as [registrator](https://github.com/gliderlabs/registrator) and [consul-template](https://github.com/hashicorp/consul-template).

## License

This project is provided compliments of [Daemon](http://www.daemon.com.au/) and made available under the [MIT license](LICENSE.txt).

The insecure SSH key is copyrighted by [Phusion](http://www.phusion.nl/) and made available under the [MIT license](image/LICENSE.txt).
