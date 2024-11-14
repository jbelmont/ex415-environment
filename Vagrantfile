# Vagrantfile for EX415 exam preparation with GUI on control node for VNC access
Vagrant.configure("2") do |config|
  # Define VM settings
  config.vm.box = "generic/centos8" # Switch to CentOS 8 for compatibility with GUI packages

  # Define managed nodes
  (1..4).each do |i|
    config.vm.define "managed#{i}" do |managed|
      managed.vm.hostname = "managed-node-#{i}"
      managed.vm.network "private_network", ip: "192.168.56.1#{i + 1}"
      managed.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
        vb.cpus = 2
      end
    end
  end

  # Define VM for the Control Node with GUI and VNC access
  config.vm.define "control" do |control|
    control.vm.hostname = "control-node"
    control.vm.network "private_network", ip: "192.168.56.10"
    control.vm.provider "virtualbox" do |vb|
      vb.memory = "4096" # Increased memory for GUI
      vb.cpus = 2
    end

    # Trigger to ensure all managed nodes are up before configuring the control node
    control.trigger.before :up do |trigger|
      trigger.info = "Ensuring all managed nodes are fully up before provisioning the control node..."
      trigger.run = { inline: "vagrant up managed1 managed2 managed3 managed4" }
    end

    # Provision the GUI and VNC server on the control node
    control.vm.provision "shell", inline: <<-SHELL
      # Enable EPEL repository and install GUI and VNC server
      yum install -y epel-release
      yum groupinstall -y "Server with GUI"
      yum install -y tigervnc-server

      # Set up VNC server configuration
      echo "VNCSERVERS=\"1:vagrant\"" >> /etc/sysconfig/vncservers
      echo "VNCSERVERARGS[1]=\"-geometry 1024x768 -localhost\"" >> /etc/sysconfig/vncservers

      # Configure VNC password for the vagrant user
      mkdir -p /home/vagrant/.vnc
      echo "vagrant" | vncpasswd -f > /home/vagrant/.vnc/passwd
      chmod 600 /home/vagrant/.vnc/passwd
      chown -R vagrant:vagrant /home/vagrant/.vnc

      # Set up the xstartup script
      cat <<EOF > /home/vagrant/.vnc/xstartup
#!/bin/sh

unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS

# Start GNOME session directly if installed
if [ -x /usr/bin/gnome-session ]; then
    exec /usr/bin/gnome-session &
else
    # Fallback to default X session if GNOME is not available
    exec /etc/X11/xinit/xinitrc &
fi
EOF

      # Make xstartup executable
      chmod +x /home/vagrant/.vnc/xstartup
      chown vagrant:vagrant /home/vagrant/.vnc/xstartup

      # Modify the VNC service file to specify the vagrant user
      cat <<SERVICE > /etc/systemd/system/vncserver@:1.service
[Unit]
Description=Remote desktop service (VNC)
After=syslog.target network.target

[Service]
Type=forking
User=vagrant
PIDFile=/home/vagrant/.vnc/%H:%i.pid
ExecStart=/usr/bin/vncserver %i -geometry 1024x768 -localhost
ExecStop=/usr/bin/vncserver -kill %i

[Install]
WantedBy=multi-user.target
SERVICE

      # Reload systemd to apply the changes
      systemctl daemon-reload

      # Enable and start the VNC server
      systemctl enable vncserver@:1
      systemctl start vncserver@:1 || (echo "VNC server failed to start. Check configuration." && exit 1)
    SHELL

    # Generate SSH key for control node to connect to managed nodes
    control.vm.provision "shell", inline: <<-SHELL
      ssh-keygen -t rsa -f /home/vagrant/.ssh/id_rsa -q -N ""
    SHELL

    # Provision to copy SSH key from control node to each managed node
    (1..4).each do |i|
      control.vm.provision "shell", inline: <<-SHELL
        # Copy the control node's SSH public key to managed-node-#{i}
        scp -o StrictHostKeyChecking=no /home/vagrant/.ssh/id_rsa.pub vagrant@192.168.56.1#{i + 1}:/home/vagrant/authorized_key_tmp
        ssh -o StrictHostKeyChecking=no vagrant@192.168.56.1#{i + 1} 'mkdir -p ~/.ssh && cat ~/authorized_key_tmp >> ~/.ssh/authorized_keys && rm ~/authorized_key_tmp'
      SHELL
    end
  end
end
