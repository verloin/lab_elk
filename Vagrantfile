BOX_UBUNTU = "generic/ubuntu2004"


######################################################################

Vagrant.configure("2") do |config|
	config.vm.box_check_update = false

  config.vm.define "ubuntu-2004" do |sub|
    sub.vm.box = "#{BOX_UBUNTU}"
    sub.vm.hostname = "ubuntu-2004"
    sub.vm.network "private_network", ip: "10.0.0.101"

    sub.vm.provision "Copying folder with configuration files to VM", type: "file", run: "once" do |vb|
      vb.source = Dir.pwd + "/"
      vb.destination = "/tmp/ubuntu"
    end

    sub.vm.provision "shell", inline: $base_prepare_script, run: "once"
    sub.vm.provision "shell", inline: $ubuntu_install_ansible, run: "once"
    sub.vm.provision "shell", inline: $ubuntu_monitoring, run: "once"

    sub.vm.provider "virtualbox" do |vb|
      vb.name = "ubuntu-2004"
      vb.memory = 8192
      vb.cpus = 2
    end
  end  
end

##################################################################
#############    SHELL   ####
##################################################################

$base_prepare_script = <<-SHELL
  sudo apt update -y && apt upgrade -y
  sudo apt install mc -y
  sudo apt install sysstat -y
  sudo apt install curl -y
  sudo apt install apt-transport-https -y
  sudo apt install software-properties-common -y
  sudo apt ca-certificates -y

  sudo mkdir /root/.ssh
  sudo touch /root/.ssh/authorized_keys

  sudo cp -r /tmp/ubuntu/ansible /home/vagrant/
  sudo cp -r /tmp/ubuntu/files /home/vagrant/
  sudo cp -r /tmp/ubuntu/docker /home/vagrant/

  sudo cat /tmp/ubuntu/keys/public_client > /root/.ssh/authorized_keys
SHELL

$ubuntu_install_ansible = <<-SHELL
  sudo apt install git python3-pip -y
  sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 2
  sudo pip3 install ansible
  
SHELL

$ubuntu_monitoring = <<-SHELL
  sudo ansible-playbook /home/vagrant/ansible/playbooks/docker_install.yml
  sudo ansible-playbook /home/vagrant/ansible/playbooks/deploy_node-exporter.yml
  sudo ansible-playbook /home/vagrant/ansible/playbooks/up_monitoring.yml
  
SHELL
