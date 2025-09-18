Vagrant.configure("2") do |config|
  config.vm.box = "generic/rhel8"
  config.vm.hostname = "techdev2"
  config.vm.network "public_network"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
  end

  # Installer Ansible dans la VM
  config.vm.provision "shell", name: "Install Ansible", inline: <<-SHELL
    set -e
    sudo dnf -y install python3 python3-pip
    sudo python3 -m pip install --upgrade pip
    sudo python3 -m pip install "ansible>=2.13,<10"
  SHELL

  # Copier le playbook dans la VM (depuis le même dossier que le Vagrantfile)
  config.vm.provision "file",
    source: "playbook-prerequis.yaml",
    destination: "/tmp/playbook-prerequis.yaml"

  # Lancer Ansible *dans* la VM en pointant sur le fichier copié
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "/tmp/playbook-prerequis.yaml"
    ansible.provisioning_path = "/tmp"
    ansible.compatibility_mode = "2.0"
    ansible.become = true
    ansible.raw_arguments = ["-e", "ansible_python_interpreter=/usr/bin/python3"]
  end
end
