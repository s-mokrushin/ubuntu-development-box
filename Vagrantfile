# -*- mode: ruby -*-
# vi: set ft=ruby :

unless File.exist?('id_rsa')
    abort "File id_rsa does not exists in directory, please add."
end

unless File.exist?('.env')
    abort "File .env does not exists in directory, please add."
end

Vagrant.configure("2") do |config|
  config.vagrant.plugins = [
    "vagrant-env",
    "vagrant-disksize",
    "vagrant-vbguest",
    "vagrant-hostsupdater"
  ]

  config.disksize.size = ENV['BOX_DISK_SIZE']

  config.vm.box = "s-mokrushin/ubuntu-20.04-desktop-amd64"
  config.vm.box_version = "1.0"
  config.vm.synced_folder "./", "/box"
  config.env.enable

  config.vm.network :private_network, ip: ENV['BOX_IP_ADDRESS']
  config.vm.hostname = ENV['BOX_HOSTNAME']

  config.vm.provider "virtualbox" do |v|
    v.gui = true
    v.memory = ENV['BOX_MEMORY_SIZE']
    v.cpus = ENV['BOX_CPU_COUNT']
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.provisioning_path = "/box"
    ansible.limit = "all"
    ansible.playbook = "dev.yml"
    ansible.install_mode = "pip"
    ansible.pip_install_cmd = "curl https://bootstrap.pypa.io/pip/2.7/get-pip.py | sudo python"
  end
end