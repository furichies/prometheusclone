Vagrant.configure("2") do |config|
  # Configuración de prometheus
  config.vm.define "prometheus" do |prometheus|
    prometheus.vm.box = "ubuntu/jammy64"
    prometheus.vm.hostname = "prometheus"
    prometheus.vm.network "private_network", ip: "192.168.53.200"
    prometheus.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end

    prometheus.vm.provision "shell", inline: <<-SHELL
      # Descarga y descompresión de Prometheus
      wget https://github.com/prometheus/prometheus/releases/download/v2.44.0-rc.0/prometheus-2.44.0-rc.0.linux-amd64.tar.gz
      tar xvf prometheus-2.44.0-rc.0.linux-amd64.tar.gz

      # Configuración de /etc/hosts
      echo "192.168.53.200 prometheus" | sudo tee -a /etc/hosts
      echo "192.168.53.201 node1" | sudo tee -a /etc/hosts
      echo "192.168.53.202 node2" | sudo tee -a /etc/hosts

      # Ejecución de Prometheus en segundo plano
      cd prometheus-2.44.0-rc.0.linux-amd64/
      cp /vagrant/prometheus.yml .
      nohup ./prometheus &
    SHELL
  end

  # Configuración de node1 y node2 mediante un bucle en Ruby
  (1..2).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "ubuntu/jammy64"
      node.vm.hostname = "node#{i}"
      node.vm.network "private_network", ip: "192.168.53.20#{i}"
      node.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
      end

      node.vm.provision "shell", inline: <<-SHELL
        # Descarga y descompresión de node_exporter
        wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
        tar xvf node_exporter-1.5.0.linux-amd64.tar.gz
        cd node_exporter-1.5.0.linux-amd64/

        # Ejecución de node_exporter en segundo plano
        nohup ./node_exporter &

        # Configuración de /etc/hosts
        echo "192.168.53.200 prometheus" | sudo tee -a /etc/hosts
        echo "192.168.53.201 node1" | sudo tee -a /etc/hosts
        echo "192.168.53.202 node2" | sudo tee -a /etc/hosts
      SHELL
    end
  end
end
