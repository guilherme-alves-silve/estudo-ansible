$script_ansible = <<-SCRIPT
  apt-get update && \
  apt-get install -y software-properties-common && \
  apt-add-repository --yes --update ppa:ansible/ansible && \
  apt-get install -y ansible
SCRIPT

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64" 

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 512
    vb.cpus = 1
  end

  #If you have problem connecting with some VMs, backup the ~/.ssh/known_hosts and remove him:
  #rm ~/.ssh/known_hosts
  #If you want to use the ansible in you machine, you can you the commands below:
  #ansible wordpress -u vagrant --private-key .vagrant/machines/wordpress/virtualbox/private_key -i hosts -m shell -a 'echo Hello, World'
  #If you want verbose information:
  #ansible -vvvv wordpress -u vagrant --private-key .vagrant/machines/wordpress/virtualbox/private_key -i hosts -m shell -a 'echo Hello, World'
  #If you have problem with ssh, do the steps below:
  #mkdir ssh-vagrant-keys
  #cd ssh-vagrant-keys
  #ssh-keygen -t rsa
  #ssh-copy-id -i vagrant_id_rsa.pub vagrant@192.168.50.2
  #cd ..
  #ansible -vvvv wordpress -u vagrant --private-key ssh-vagrant-keys/vagrant_id_rsa -i hosts -m shell -a 'echo Hello, World'
  #Execute the playbook:
  #ansible-playbook provisioning.yml -u vagrant -i hosts --private-key .vagrant/machines/wordpress/virtualbox/private_key
  #If you want to modify the variables value, do the command below:
  #ansible-playbook provisioning.yml -i hosts --extra-vars "wp_db=wordpress_db wp_username=phpuser"
  #Because of the complete configuration in the file hosts, you just do the command below:
  #ansible-playbook provisioning.yml -i hosts
  config.vm.define "ansible" do |ansible|
  	ansible.vm.network "private_network", ip: "192.168.50.1"
  	ansible.vm.provision "shell", inline: $script_ansible
  	ansible.vm.provision "shell", inline: "ansible-playbook /vagrant/provisioning.yml -i hosts"

    ansible.vm.provider "virtualbox" do |vb|
      vb.name = "ansible_vm"
    end
  end

  config.vm.define "wordpress" do |wordpress|
	wordpress.vm.network "private_network", ip: "192.168.50.2"

    wordpress.vm.provider "virtualbox" do |vb|
      vb.name = "wordpress_vm"
    end
  end

  config.vm.define "database" do |database|
    database.vm.network "private_network", ip: "192.168.50.3"

    database.vm.provider "virtualbox" do |vb|
      vb.name = "database_vm"
    end
  end

end
