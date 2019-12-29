Vagrant.configure("2") do |config|

  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "generic/fedora31"
  config.vm.hostname = "station"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "station"
    vb.cpus = 2
    vb.gui = true
    vb.memory = "6144"
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ["modifyvm", :id, "--clipboard-mode", "bidirectional"]
  end

  config.vm.provision :shell, :inline => <<-SHELL
  sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
  sestatus | grep --silent "SELinux status:.*disabled"
  if [ $? -ne 0 ]; then
    setenforce 0
  fi
SHELL

  config.vm.provision "ansible_local" do |ansible|
  	ansible.playbook = "playbook.yml"
  end

end
