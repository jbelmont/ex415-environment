# Vagrantfile for EX415 exam preparation with no pre-installed tools on either node
Vagrant.configure("2") do |config|
  # Define VM settings
  config.vm.box = "generic/rhel8" # Use "generic/rhel8" for Red Hat-like environment

  # Define VM for the Control Node
  config.vm.define "control" do |control|
    control.vm.hostname = "control-node"
    control.vm.network "private_network", ip: "192.168.56.10"
    control.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = 2
    end

    # Generate SSH key for control node to connect to managed nodes
    control.vm.provision "shell", inline: <<-SHELL
      ssh-keygen -t rsa -f /home/vagrant/.ssh/id_rsa -q -N ""
    SHELL
  end

  # Define VM for the Managed Node
  config.vm.define "managed" do |managed|
    managed.vm.hostname = "managed-node"
    managed.vm.network "private_network", ip: "192.168.56.11"
    managed.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = 2
    end

    # No pre-installed tools; leave configurations open for manual installation
  end

  # Provision to copy SSH key from control node to managed node
  config.vm.provision "shell", inline: <<-SHELL
    # Transfer the control node's SSH public key to the managed node
    sshpass -p vagrant ssh-copy-id -o StrictHostKeyChecking=no -i /home/vagrant/.ssh/id_rsa.pub vagrant@192.168.56.11
  SHELL
end
