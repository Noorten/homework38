Vagrant.configure("2") do |config|
  # Указываем ОС, версию, количество ядер и ОЗУ
  config.vm.box = "generic/centos8s"
  config.vm.box_version = "4.2.16"

  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 1
  end

  # Указываем имена хостов и их IP-адреса
  boxes = [
    { :name => "ipa.otus.lan",
      :ip => "192.168.57.10",
    },
    { :name => "client1.otus.lan",
      :ip => "192.168.57.11",
    },
    { :name => "client2.otus.lan",
      :ip => "192.168.57.12",
    }
  ]
    # Цикл запуска виртуальных машин

  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network "private_network", ip: opts[:ip]
      config.vm.provision "shell", inline: <<-SHELL
         mkdir -p ~root/.ssh
         cp ~vagrant/.ssh/auth* ~root/.ssh
         sed -i.bak 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
         sed -i.bak 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
         sed -i 's/^#PasswordAuthentication.*$/PasswordAuthentication yes/' /etc/ssh/sshd_config
         systemctl restart sshd.service
      SHELL

      if opts[:name] == "client2.otus.lan"
        config.vm.provision "ansible" do |ansible|
          ansible.playbook = "ansible/provision.yml"
          ansible.inventory_path = "ansible/hosts"
          ansible.host_key_checking = "false"
          ansible.become = "true"
          ansible.limit = "all"
        end
      end
    end
  end
end
