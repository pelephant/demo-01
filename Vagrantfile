Vagrant.configure("2") do |config|
  config.vm.network "forwarded_port", guest: 9100, host: 9100
  config.vm.network "forwarded_port", guest: 9090, host: 9090
  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.box = "ubuntu2004"
config.vm.provider "virtualbox" do |v|
  v.memory = 4096
  v.cpus = 4
end
config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
end
end
