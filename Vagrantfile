Vagrant.configure("2") do |config|
  
  # Imposta la box da utilizzare
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.provider "vmware_desktop"

  # Configurazione per il nodo master/worker
  config.vm.define "master-worker" do |node|
    node.vm.hostname = "master-worker"
    node.vm.network "private_network", ip: "192.168.56.10"
    node.vm.provider "vmware_desktop" do |v|
      v.memory = 2048
      v.cpus = 2
    end

    node.vm.provision "shell", inline: <<-SHELL
      echo "Configurazione di master-worker..."
      sudo apt-get update -y
      sudo apt-get install -y python3 python3-pip
      sudo pip3 install --no-cache-dir --upgrade pip setuptools
      sudo apt-get install -y ansible
      # Altri pacchetti necessari per Kubespray
    SHELL
  end

  # Configurazione per il nodo worker
  config.vm.define "worker-only" do |node|
    node.vm.hostname = "worker-only"
    node.vm.network "private_network", ip: "192.168.56.11"
    node.vm.provider "vmware_desktop" do |v|
      v.memory = 2048
      v.cpus = 2
    end

    node.vm.provision "shell", inline: <<-SHELL
      echo "Configurazione di worker-only..."
      sudo apt-get update -y
      sudo apt-get install -y python3 python3-pip
      sudo pip3 install --no-cache-dir --upgrade pip setuptools
      sudo apt-get install -y ansible
      # Altri pacchetti necessari per Kubespray
    SHELL
  end

  # Configurazione del file hosts.ini per Kubespray
  config.vm.provision "shell", inline: <<-SHELL
    cd /vagrant/kubespray
    cp -rfp inventory/sample inventory/mycluster
    CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py 192.168.56.10 192.168.56.11
  SHELL

end

