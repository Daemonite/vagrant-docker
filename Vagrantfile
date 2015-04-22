# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.require_version ">= 1.4"

ROOT = File.dirname(File.absolute_path(__FILE__))

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'

# Default env properties which can be overridden
# Example overrides:
#   echo "ENV['PASSENGER_DOCKER_PATH'] ||= '../../phusion/passenger-docker'   " >> ~/.vagrant.d/Vagrantfile
#   echo "ENV['BASE_BOX_URL']          ||= 'd\:/dev/vm/vagrant/boxes/phusion/'" >> ~/.vagrant.d/Vagrantfile
BASE_BOX_URL          = ENV['BASE_BOX_URL']    || 'https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/'
VAGRANT_BOX_URL       = ENV['VAGRANT_BOX_URL'] || BASE_BOX_URL + 'ubuntu-14.04-amd64-vbox.box'
VMWARE_BOX_URL        = ENV['VMWARE_BOX_URL']  || BASE_BOX_URL + 'ubuntu-14.04-amd64-vmwarefusion.box'
DOCKERHOST_MEMSIZE    = ENV['DOCKERHOST_MEMSIZE'] || '1024'
DOCKERHOST_IPADDR     = ENV['DOCKERHOST_IPADDR'] || ''
DOCKER_FWDPORT_MIN    = ENV['DOCKER_FWDPORT_MIN'] || '48000'
DOCKER_FWDPORT_MAX    = ENV['DOCKER_FWDPORT_MAX'] || '48199'

DOCKER_COMPOSE_VERSION = ENV['DOCKER_COMPOSE_VERSION'] || '1.2.0'
DOCKER_COMPOSE_EXE     = 'docker-compose'

$script = <<SCRIPT
cat /vagrant/bash_aliases.sh >> /home/vagrant/.bash_aliases
echo -e "\\nfunction dsh { docker exec -ti \\"\\$1\\" bash -l; }" >> /home/vagrant/.bashrc
mkdir -p /home/vagrant/.ssh
cp /vagrant/image/insecure_key /home/vagrant/.ssh/id_rsa
chown -R vagrant.vagrant /home/vagrant/.bash_aliases /home/vagrant/.ssh
chmod -R go= /home/vagrant/.ssh
if [ ! -f /vagrant/tools/docker-compose ]; then
  mkdir -p /vagrant/tools
  wget --no-check-certificate https://github.com/docker/compose/releases/download/#{DOCKER_COMPOSE_VERSION}/docker-compose-Linux-x86_64 -O /vagrant/tools/docker-compose
fi
cp /vagrant/tools/docker-compose /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = 'phusion/ubuntu-14.04-amd64'
  config.vm.box_url = VAGRANT_BOX_URL
  config.ssh.forward_agent = true

  if DOCKERHOST_IPADDR != ''
    web.vm.network :private_network, ip: DOCKERHOST_IPADDR
  end

  config.vm.provider :virtualbox do |v|
	  v.memory = DOCKERHOST_MEMSIZE
  end

  config.vm.provider :vmware_fusion do |f, override|
    override.vm.box_url = VMWARE_BOX_URL
    f.vmx['displayName'] = 'baseimage-docker'
    f.vmx['memsize'] = DOCKERHOST_MEMSIZE
  end

  if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/default/*/id").empty?
    config.vm.provision :shell, :inline => $script
  end

  config.vm.provision :docker do |d|
  end

  # Port range for use by Docker containers to publish on the VM host
  (DOCKER_FWDPORT_MIN..DOCKER_FWDPORT_MAX).each do |port|
    config.vm.network :forwarded_port, :host => port, :guest => port
  end
end
