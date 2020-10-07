### TP2 Linux B2 ###
 
 
:::info 
**Vagrantfile - Multi-node deployment**
Pour créer et configurer node1 et node2
 ```VVagrant.configure("2") do |config|
## All VM's basic config attribute ##
  config.vm.box = "b2-tp2-centoq"
##Erreur de nom pour la box au-dessus déso

  config.vbguest.auto_update = false
  config.vm.box_check_update = false
  config.vm.synced_folder ".", "/vagrant", disabled: true

  ## NODE1 creation / config / ip / mem / name ##
  config.vm.define "node1" do |node1|
    node1.vm.network "private_network", ip: "192.168.2.11", mask: "255.255.255.0"
    node1.vm.hostname = "node1.tp2.b2"
    #node1.vm.provision "shell", path: "my_script/boot_script1.sh"
    node1.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
      vb.name = "node1.tp2.b2"
    end
  end

  ##    NODE2 creation / config / ip / mem / name ##
  config.vm.define "node2" do |node2|
    node2.vm.network "private_network", ip: "192.168.2.12", mask: "255.255.255.0"
    node2.vm.hostname = "node2.tp2.b2"
   # node2.vm.provision "shell", path: "my_script/boot_script2.sh"
    node2.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
      vb.name = "node2.tp2.b2"
    end
  end
end
:::


:::info
**myscript/script_boot1.sh**
```

#!bin/bash
#User
useradd admin -m 
usermod aG- wheel admin

#Selinux 
selinux setenforce 0
sed -i 's/SELINUX=enforce/SELINUX=disabled' /etc/selinux/config

#Firewall
systemctl start
firewalld-cmd --set-default-zone block
firewalld-cmd --add-port=80/tcp --permanent
firewalld-cmd --add-port=443/tcp --permanent
firewalld-cmd --reload
```

:::

:::info
**myscript/script_boot2.sh**

```#!bin/bash
#User
useradd admin -m 
usermod aG- wheel admin

#Selinux 
selinux setenforce 0

#Firewall
systemctl start
firewalld-cmd --set-default-zone block
firewalld-cmd --add-port=80/tcp --permanent
firewalld-cmd --add-port=443/tcp --permanent
firewalld-cmd --reload
:::

:::success
**Vagrantfile pour Autodéploiement**
 ```
 Vagrant.configure("2") do |config|

  ## All VM's basic config attribute ##
  config.vm.box = "b2-tp2-centoq"

  config.vbguest.auto_update = false
  config.vm.box_check_update = false 
  config.vm.synced_folder ".", "/vagrant", disabled: true

  ## NODE1 creation / config / ip / mem / name ##
  config.vm.define "node1" do |node1|
    node1.vm.network "private_network", ip: "192.168.2.11", mask: "255.255.255.0"
    node1.vm.hostname = "node1.tp2.b2"
    node1.vm.provision "shell", path: "myscript/script_boot1.sh"
    node1.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
      vb.name = "node1.tp2.b2"
    end
  end

  ## 	NODE2 creation / config / ip / mem / name ##
  config.vm.define "node2" do |node2|
    node2.vm.network "private_network", ip: "192.168.2.12", mask: "255.255.255.0"
    node2.vm.hostname = "node2.tp2.b2"
   # node2.vm.provision "shell", path: "myscript/script_boot2.sh"
    node2.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
      vb.name = "node2.tp2.b2"
    end
  end
end
```

:::