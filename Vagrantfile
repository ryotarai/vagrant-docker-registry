# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.network "private_network", type: 'dhcp'

  config.vm.provision(:shell, inline: <<-EOC)
set -e

cat /etc/apt/sources.list | sed -e 's|http://[^ ]*|mirror://mirrors.ubuntu.com/mirrors.txt|g' > /tmp/sources.list
mv /tmp/sources.list /etc/apt/sources.list

# Install docker
if ! (which docker); then
  [ -e /usr/lib/apt/methods/https ] || {
    apt-get -y update
    apt-get -y install apt-transport-https
  }

  apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
  echo 'deb https://get.docker.com/ubuntu docker main' > /etc/apt/sources.list.d/docker.list
  apt-get -y update
  apt-get -y install lxc-docker
fi

if ! (docker ps | grep -q registry); then
  docker run \
  -e SETTINGS_FLAVOR=s3 \
  -e AWS_BUCKET=#{ENV['DOCKER_REGISTRY_AWS_BUCKET']} \
  -e STORAGE_PATH=#{ENV['DOCKER_REGISTRY_STORAGE_PATH']} \
  -e AWS_KEY=#{ENV['AWS_ACCESS_KEY_ID']} \
  -e AWS_SECRET=#{ENV['AWS_SECRET_ACCESS_KEY']} \
  -e SEARCH_BACKEND=sqlalchemy \
  -p 80:5000 \
  -d \
  registry
fi
  EOC
end
