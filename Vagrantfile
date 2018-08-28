$sudovagrant = <<SCRIPT
usermod -aG sudo vagrant
SCRIPT

$installdocker = <<SCRIPT
sudo apt-get update
sudo apt-get install -y curl
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce
curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
SCRIPT

$installjava = <<SCRIPT
sudo apt-get update
sudo apt-get install -y default-jdk
sudo apt-get install -y maven
SCRIPT

Vagrant.configure("2") do |config|

 config.vm.provider "virtualbox" do |v|
    v.linked_clone = true
    v.customize ["modifyvm", :id, "--memory", "512", "--cpus", "1"]
 end

 config.vm.define "manager" do |mg|
   mg.vm.box = "ubuntu/xenial64"
   mg.vm.hostname = "manager"
   mg.vm.network :private_network, ip: "192.168.10.10"
   mg.vm.provision "shell", inline: $sudovagrant
   mg.vm.provision "shell", inline: $installdocker 
   mg.vm.provision "shell", inline: $installjava
#   mg.vm.provision "shell", inline: "usermod -aG docker vagrant"
   mg.vm.provider "virtualbox" do |pmv|
      pmv.memory = 2048
      pmv.cpus = 1
   end
 end
 
 config.vm.define "worker1" do |w1|
   w1.vm.box = "ubuntu/xenial64"
   w1.vm.hostname = "worker-1"
   w1.vm.network :private_network, ip: "192.168.10.20"
   w1.vm.provision "shell", inline: $sudovagrant 
   w1.vm.provision "shell", inline: $installdocker 
   w1.vm.provision "shell", inline: $installjava 
   w1.vm.provision "shell", inline: "usermod -aG docker vagrant"
 end

 config.vm.define "worker2" do |w2|
   w2.vm.box = "ubuntu/xenial64"
   w2.vm.hostname = "worker-2"
   w2.vm.network :private_network, ip: "192.168.10.30"
   w2.vm.provision "shell", inline: $sudovagrant 
   w2.vm.provision "shell", inline: $installdocker 
   w2.vm.provision "shell", inline: $installjava
   w2.vm.provision "shell", inline: "usermod -aG docker vagrant"
 end

end