# -*- mode: ruby -*-
# vi: set ft=ruby :
$hostsfile_update = <<-'SCRIPT'
echo -e '192.168.56.110 control.example.com control\n192.168.56.111 node1.example.com node1\n192.168.56.112 node2.example.com node2' >> /etc/hosts
sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config && systemctl restart sshd
sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
SCRIPT

$control_first_setup = <<-'SCRIPT2'
sudo yum -y install epel-release
sudo yum -y install ansible
pip3 install ansible-lint "ansible>=2.9,<2.10"
ssh-keygen -t rsa -f ~/.ssh/id_rsa -q -P ""
sshpass -p "vagrant" ssh-copy-id -i ~/.ssh/id_rsa.pub -o StrictHostKeyChecking=no root@192.168.56.111
sshpass -p "vagrant" ssh-copy-id -i ~/.ssh/id_rsa.pub -o StrictHostKeyChecking=no root@192.168.56.112
SCRIPT2

Vagrant.configure("2") do |config|

  config.vm.define "control", primary: true do |control|
    control.vm.box = "centos/8"
    control.vm.hostname = "control.example.com"
    control.vm.network "forwarded_port", guest: 80, host: 8080
    control.vm.network "private_network", ip: "192.168.56.110"
    control.vm.provision "shell", inline: $hostsfile_update
    # control.vm.provision "shell", inline: $control_install_ansible
    control.vm.provider "virtualbox" do |v|
      config.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--audio", "none"]
      end
      # control.vm.synced_folder "C:\\Projects\\lesson-ansible-01\\ansible", "/ansible", owner: "root", group: "root"
      v.memory = 4096
      v.cpus = 2
    end
  end

  config.vm.define "node1" do |node1|
    node1.vm.box = "centos/8"
    node1.vm.hostname = "node1.example.com"
    node1.vm.network "private_network", ip: "192.168.56.111"
    node1.vm.provision "shell", inline: $hostsfile_update
    config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--audio", "none"]
    end
  end

  config.vm.define "node2" do |node2|
    node2.vm.box = "centos/8"
    node2.vm.hostname = "node2.example.com"
    node2.vm.network "private_network", ip: "192.168.56.112"
    node2.vm.provision "shell", inline: $hostsfile_update
    config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--audio", "none"]
    end
  end
  config.vbguest.auto_update = false
end