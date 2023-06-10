
# -*- mode: ruby -*-
# vi: set ft=ruby :
# vagrant plugin install vagrant-vbguest
# vagrant plugin install vagrant-disksize
# vagrant box add peru-ubuntu-2004-desktop file:///C:/VAGRANT_all/Boxes/peru-ubuntu-20.04-desktop-amd64

BOX_DEBIAN = "debian-buster64"
# BOX_DEBIAN_DESKTOP = "boxomatic/debian-10-mate"
BOX_UBUNTU_DESKTOP = "ubuntu-2004-desktop"
# BOX_VERSION_UBUNTU_DESKTOP = "20211016.01"
BOX_UBUNTU_SERVER = "ubuntu-2004-server"

# $hosts
#       10.0.0.100 logstash
#       10.0.0.101 elastic-01
#       10.0.0.102 elastic-02
#       10.0.0.103 elastic-03
#       10.0.0.104 kibana
#       10.0.0.105 jenkins
#       10.0.0.106 postgresql
#       10.0.0.107 gitlab
#       10.0.0.108 docker
#       10.0.0.109 prometheus\grafana
#       10.0.0.110 ubuntu (ansible root)
#       10.0.0.111 bitbucket
#       10.0.0.112 kubernetes
#       10.0.0.113 kafka01
#       10.0.0.114 kafka02
#       10.0.0.115 kafka03
#       10.0.0.116 nexus
#
#       10.0.0.201 node-01
#       10.0.0.202 node-02


######################################################################

Vagrant.configure("2") do |config|
	config.vm.box_check_update = false
  # config.disksize.size = "50GB"

  ###################################################################
  (1..3).each do |i|
    config.vm.define "kafka-0#{i}" do |node|
      node.vm.box = "#{BOX_DEBIAN}"
      node.vm.hostname = "kafka-0#{i}"
      node.vm.network "private_network",ip: "10.0.0.#{112+i}"
      node.vm.provision "shell", inline: $base_script, run: "once"
      node.vm.provision "shell", inline: $add_hosts, run: "once"

      node.vm.provider "virtualbox" do |vb|
        vb.name = "kafka-0#{i}"
        vb.memory = 2048
        vb.cpus = 1
      end
    end
  end

  ####################################################################
  config.vm.define "nexus" do |sub|
    sub.vm.box = "#{BOX_DEBIAN}"
    sub.vm.hostname = "nexus"
    sub.vm.network "private_network", ip: "10.0.0.116"

    sub.vm.provision "shell", inline: $base_script, run: "once"
    sub.vm.provision "shell", inline: $add_hosts, run: "once"
  
    sub.vm.provider "virtualbox" do |vb|
      vb.name = "nexus"
      vb.memory = 4096
      vb.cpus = 1
    end
  end

  ####################################################################
  config.vm.define "logstash" do |sub|
    sub.vm.box = "#{BOX_DEBIAN}"
    sub.vm.hostname = "logstash"
    sub.vm.network "private_network", ip: "10.0.0.100"

    sub.vm.provision "shell", inline: $base_script, run: "once"
    sub.vm.provision "shell", inline: $add_hosts, run: "once"
  
    sub.vm.provider "virtualbox" do |vb|
      vb.name = "logstash"
      vb.memory = 4096
      vb.cpus = 1
    end
  end

  ####################################################################
  (1..3).each do |i|
    config.vm.define "elastic-0#{i}" do |node|
      node.vm.box = "#{BOX_DEBIAN}"
      node.vm.hostname = "elastic-0#{i}"
      node.vm.network "private_network",ip: "10.0.0.#{100+i}"

      node.vm.provision "shell", inline: $base_script, run: "once"
      node.vm.provision "shell", inline: $add_hosts, run: "once"

      node.vm.provider "virtualbox" do |vb|
        vb.name = "elastic-0#{i}"
        vb.memory = 4096
        vb.cpus = 1
      end
    end
  end

  ####################################################################
  config.vm.define "kibana" do |sub|
    sub.vm.box = "#{BOX_DEBIAN}"
    sub.vm.hostname = "kibana"
    sub.vm.network "private_network", ip: "10.0.0.104"

    sub.vm.provision "shell", inline: $base_script, run: "once"
    sub.vm.provision "shell", inline: $add_hosts, run: "once"
  
    sub.vm.provider "virtualbox" do |vb|
      vb.name = "kibana"
      vb.memory = 2048
      vb.cpus = 1
    end
  end

  ###################################################################
  (1..3).each do |i|
    config.vm.define "node-0#{i}" do |node|
      node.vm.box = "#{BOX_DEBIAN}"
      node.vm.hostname = "node-0#{i}"
      node.vm.network "private_network",ip: "10.0.0.#{200+i}"
      node.vm.provision "shell", inline: $base_script, run: "once"
      node.vm.provision "shell", inline: $add_hosts, run: "once"

      node.vm.provider "virtualbox" do |vb|
        vb.name = "node-0#{i}"
        vb.memory = 2048
        vb.cpus = 1
      end
    end
  end

  # ####################################################################
  # config.vm.define "postgresql" do |sub|
  #   sub.vm.box = "#{BOX_DEBIAN}"
  #   sub.vm.hostname = "postgresql"
  #   sub.vm.network "private_network", ip: "10.0.0.106"

  #   sub.vm.provision "shell", inline: $base_script, run: "once"
  #   sub.vm.provision "shell", inline: $add_hosts, run: "once"
  
  #   sub.vm.provider "virtualbox" do |vb|
  #     vb.name = "postgresql"
  #     vb.memory = 2048
  #     vb.cpus = 1
  #   end
  # end

  ####################################################################
  config.vm.define "prometheus" do |sub|
    sub.vm.box = "#{BOX_DEBIAN}"
    sub.vm.hostname = "prometheus"
    sub.vm.network "private_network", ip: "10.0.0.109"

    sub.vm.provision "shell", inline: $base_script, run: "once"
    sub.vm.provision "shell", inline: $add_hosts, run: "once"
    sub.vm.provision "shell", inline: $install_ANSIBLE, run: "once" 

    sub.vm.provider "virtualbox" do |vb|
      vb.name = "prometheus"
      vb.memory = 8192
      vb.cpus = 4
    end
  end

  ####################################################################
  config.vm.define "docker" do |sub|
    sub.vm.box = "#{BOX_DEBIAN}"
    sub.vm.hostname = "docker"
    sub.vm.network "private_network", ip: "10.0.0.108"

    sub.vm.provision "shell", inline: $base_script, run: "once"
    sub.vm.provision "shell", inline: $add_hosts, run: "once"
    sub.vm.provision "shell", inline: $install_docker, run: "once"

    sub.vm.provider "virtualbox" do |vb|
      vb.name = "docker"
      vb.memory = 16384
      vb.cpus = 4
    end
  end

  ####################################################################
  config.vm.define "jenkins" do |sub|
    sub.vm.box = "#{BOX_DEBIAN}"
    sub.vm.hostname = "jenkins"
    sub.vm.network "private_network", ip: "10.0.0.105"

    sub.vm.provision "shell", inline: $base_script, run: "once"
    sub.vm.provision "shell", inline: $add_hosts, run: "once"
    # sub.vm.provision "shell", inline: $install_JENKINS, run: "once"

    sub.vm.provider "virtualbox" do |vb|
      vb.name = "jenkins"
      vb.memory = 6144
      vb.cpus = 2
    end
  end

  ####################################################################
  config.vm.define "ubuntu" do |sub|
    sub.vm.box = "#{BOX_UBUNTU_DESKTOP}"
    sub.vm.hostname = "ubuntu"
    sub.vm.network "private_network", ip: "10.0.0.110"

    sub.vm.provision "Copying folder with configuration files to VM", type: "file", run: "once" do |vb|
      vb.source = Dir.pwd + "/"
      vb.destination = "/tmp/ubuntu"
    end

    sub.vm.provision "shell", inline: $ubuntu_base, run: "once"
    sub.vm.provision "shell", inline: $ubuntu_ansible, run: "once"
    sub.vm.provision "shell", inline: $add_hosts, run: "once"

    sub.vm.provider "virtualbox" do |vb|
      vb.name = "ubuntu"
      vb.memory = 8192
      vb.cpus = 4
    end
  end  
end

##################################################################
#############    SHELL   ####
##################################################################

$base_script = <<-SHELL
  sudo apt update -y && apt upgrade -y
  sudo apt install software-properties-common -y
  sudo apt install mc -y
  sudo apt install sysstat -y
  sudo apt install curl -y
  sudo apt install apt-transport-https -y
  sudo apt install wget -y
  sudo apt install default-jre -y
  sudo apt install default-jdk -y
  sudo apt install ca-certificates -y
  sudo apt install gnupg2 -y
  
  sudo mkdir /root/.ssh
  sudo touch /root/.ssh/authorized_keys

  sudo cat /vagrant/keys/public_client > /root/.ssh/authorized_keys
  sudo echo "\n" >> /root/.ssh/authorized_keys
  sudo cat /vagrant/ansible/public_ansible >> /root/.ssh/authorized_keys
  sudo echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
  sudo echo "alias ll='ls $LS_OPTIONS -l'" >> /root/.bashrc
  sudo echo "alias l='ls $LS_OPTIONS -lA'" >> /root/.bashrc
  sudo echo "alias ls='ls --color=auto'" >> /root/.bashrc
SHELL

$add_hosts = <<-SHELL
  sudo echo "127.0.0.1       localhost" > /etc/hosts
  sudo echo "127.0.0.2       buster" >> /etc/hosts
  sudo echo "::1             localhost ip6-localhost ip6-loopback" >> /etc/hosts
  sudo echo "ff02::1         ip6-allnodes" >> /etc/hosts
  sudo echo "ff02::2         ip6-allrouters" >> /etc/hosts

  sudo echo "127.0.1.1 $HOSTNAME $HOSTNAME" >> /etc/hosts
  sudo echo "10.0.0.100 logstash" >> /etc/hosts
  sudo echo "10.0.0.101 elastic-01" >> /etc/hosts
  sudo echo "10.0.0.102 elastic-02" >> /etc/hosts
  sudo echo "10.0.0.103 elastic-03" >> /etc/hosts
  sudo echo "10.0.0.104 kibana" >> /etc/hosts
  sudo echo "10.0.0.105 jenkins" >> /etc/hosts
  sudo echo "10.0.0.106 postgresql" >> /etc/hosts
  sudo echo "10.0.0.107 gitlab" >> /etc/hosts
  sudo echo "10.0.0.108 docker" >> /etc/hosts
  sudo echo "10.0.0.109 prometheus" >> /etc/hosts
  sudo echo "10.0.0.110 ubuntu" >> /etc/hosts
  sudo echo "10.0.0.111 bitbucket" >> /etc/hosts
  sudo echo "10.0.0.112 kubernetes" >> /etc/hosts
  sudo echo "10.0.0.113 kafka-01" >> /etc/hosts
  sudo echo "10.0.0.114 kafka-02" >> /etc/hosts
  sudo echo "10.0.0.115 kafka-03" >> /etc/hosts
  sudo echo "10.0.0.116 nexus" >> /etc/hosts
SHELL

$ubuntu_base = <<-SHELL
  sudo apt update -y && apt upgrade -y
  sudo apt install mc -y
  sudo apt install sysstat -y
  sudo apt-get install virtualbox-guest-x11 -y
  sudo apt install snapd -y
  sudo apt install apt-transport-https -y
  sudo snap install --classic code

  mkdir /home/vagrant/.ssh
  
  sudo cp -r /tmp/ubuntu/keys/public_client /root/.ssh/authorized_keys
  cp -r /tmp/ubuntu/keys/public_client /home/vagrant/.ssh/authorized_keys

  sudo echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
  sudo systemctl restart ssh.service
SHELL

$ubuntu_ansible = <<-SHELL
  sudo apt install software-properties-common
  sudo apt-add-repository ppa:ansible/ansible
  sudo apt update
  sudo apt install ansible -y
  sudo apt install git python3-pip -y

  mkdir /home/vagrant/ansible /home/vagrant/ansible/group_vars /home/vagrant/ansible/playbooks /home/vagrant/ansible/roles

  cp -r /tmp/ubuntu/ansible /home/vagrant/ansible
  cp -r /tmp/ubuntu/ansible/public_ansible /home/vagrant/.ssh/
  cp -r /tmp/ubuntu/ansible/privat_ansible /home/vagrant/.ssh/
SHELL

$install_docker = <<-SHELL
  sudo apt update -y && apt upgrade -y

  sudo curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
  sudo apt update

  sudo apt install docker-ce -y
  sudo usermod -aG docker root
  sudo usermod -aG docker vagrant

  sudo curl -L https://github.com/docker/compose/releases/download/1.25.3/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
  sudo chmod +x /usr/local/bin/docker-compose
SHELL

$install_JENKINS = <<-SHELL
  sudo apt-get install default-jdk -y
  sudo wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
  sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  sudo apt-get update -y
  sudo apt-get install jenkins -y
  sudo systemctl start jenkins && sudo systemctl enable jenkins
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
SHELL

$install_ANSIBLE = <<-SHELL
  sudo apt install git python3-pip -y
  sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 2
  sudo pip3 install ansible
SHELL


##  FILEBEAT
# wget http://elasticrepo.serveradmin.ru/pool/main/f/filebeat/filebeat-8.5.2-amd64.deb
# sudo dpkg -i filebeat-8.5.2-amd64.deb
# systemctl enable filebeat.service && systemctl start filebeat.service
# systemctl status filebeat.service
#
# ------------------------------------------------
#
##  KAFKA
# cd /tmp/ && wget https://downloads.apache.org/kafka/3.3.2/kafka_2.13-3.3.2.tgz
# tar -C /opt/kafka -xvf /tmp/kafka_2.13-3.3.2.tgz --strip-components 1
#
# ------------------------------------------------
#
##  ANSIBLE
#
# sudo apt install git python3-pip
# sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 2
# sudo pip3 install ansible