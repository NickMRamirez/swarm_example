# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  num_servers = 2

  vm_prereqs = %Q{
    echo "----Disabling apache----"
    sudo update-rc.d -f apache2 remove && sudo systemctl stop apache2  
  }

  install_docker = %Q{
    if [ ! $(which docker) ]; then
      sudo apt update
      sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
      curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
      sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
      sudo apt update
      sudo apt install -y docker-ce
      sudo curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
      sudo chmod +x /usr/local/bin/docker-compose
    else
      echo "docker already installed."
    fi
  }

  # init_swarm_master = %Q{
  #   sudo docker swarm init --advertise-addr 192.168.2.3
  #   sudo docker swarm join-token manager
  # }

  # init_swarm_worker = %Q{
  #   sudo docker swarm join --token #{token} 192.168.2.3:2377
  #   sudo docker stack deploy --compose-file ./docker-compose.yml web
  # }

  (1..num_servers).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "ubuntu/xenial64"
      node.vm.hostname = "node#{i}"
      node.vm.network "private_network", ip: "192.168.2.#{2 + i}"
      node.vm.provider :virtualbox do |vb|
        vb.customize ['modifyvm', :id, '--memory', '2048']
      end

      node.vm.provision :shell, inline: "sed 's/127\.0\.0\.1.*node.*/192\.168\.2\.#{2 + i} node#{i}/' -i /etc/hosts"
      # node.vm.provision "shell", inline: vm_prereqs
      node.vm.provision "shell", inline: install_docker
    end
  end

  
end
