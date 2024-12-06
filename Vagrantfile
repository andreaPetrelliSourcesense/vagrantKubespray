Vagrant.configure("2") do |config|
  # Imposta la box da utilizzare
  config.vm.box = "apetrelli/rockylinux-9"
  config.vm.box_version="1.0.0"
  config.vm.provider "vmware_desktop"

  # Configurazione della cartella condivisa HGFS
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "./sharedFolder", "/home/vagrant/sharedFolder", create: true


  # Configurazione per il nodo master/worker
  config.vm.define "master-worker" do |master|
    master.vm.hostname = "master-worker"
    master.vm.network "private_network", ip: "192.168.56.10"
    master.vm.provider "vmware_desktop" do |v|
      v.memory = 16384
      v.cpus = 2
    # Copia della chiave pubblica
    #node.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "/home/vagrant/.ssh/authorized_keys"
    end

    master.vm.provision "file", source: "~/.ssh/id_ed25519.pub", destination: "/home/vagrant/id_ed25519.pub"
    master.vm.provision "shell", inline: <<-SHELL
      cat /home/vagrant/id_ed25519.pub >> /home/vagrant/.ssh/authorized_keys
      chmod 600 /home/vagrant/.ssh/authorized_keys
      rm /home/vagrant/id_ed25519.pub
    SHELL

    master.vm.provision "shell", inline: <<-SHELL
      echo "Configurazione di master-worker..."
      sudo dnf update -y
      sudo dnf install -y python3 python3-pip
      sudo pip3 install --no-cache-dir --upgrade pip setuptools
      sudo dnf install -y epel-release
      sudo dnf install -y ansible
      # Altri pacchetti necessari per Kubespray
    SHELL
  end

  # Configurazione per il nodo worker
  config.vm.define "worker-only" do |worker|
    worker.vm.hostname = "worker-only"
    worker.vm.network "private_network", ip: "192.168.56.11"
    worker.vm.provider "vmware_desktop" do |v|
      v.memory = 16384
      v.cpus = 2
    # Copia della chiave pubblica
    #node.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "/home/vagrant/.ssh/authorized_keys"
    end

    worker.vm.provision "file", source: "~/.ssh/id_ed25519.pub", destination: "/home/vagrant/id_ed25519.pub"
    worker.vm.provision "shell", inline: <<-SHELL
      cat /home/vagrant/id_ed25519.pub >> /home/vagrant/.ssh/authorized_keys
      chmod 600 /home/vagrant/.ssh/authorized_keys
      rm /home/vagrant/id_ed25519.pub
    SHELL

    worker.vm.provision "shell", inline: <<-SHELL
      echo "Configurazione di worker-only..."
      sudo dnf update -y
      sudo dnf install -y python3 python3-pip
      sudo pip3 install --no-cache-dir --upgrade pip setuptools
      sudo dnf install -y epel-release
      sudo dnf install -y ansible
      # Altri pacchetti necessari per Kubespray
    SHELL
  end

end
