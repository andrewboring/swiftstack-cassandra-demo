# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "files", "/tmp/vagrant", type: "rsync"
  config.vm.provision "shell", inline: <<-SHELL
      #yum -y update
      #yum -y install net-tools rng-tools
      #systemctl enable rngd; systemctl start rngd
      cat /tmp/vagrant/hosts >> /etc/hosts
      mkdir /root/.ssh
      chmod 700 /root/.ssh
      mv /tmp/vagrant/id_rsa /root/.ssh/id_rsa
      chown root:root /root/.ssh/id_rsa
      chmod 600 /root/.ssh/id_rsa
      cat /tmp/vagrant/id_rsa.pub >> /root/.ssh/authorized_keys
      chmod 600 /root/.ssh/authorized_keys
      yum -y install centos-release-openstack-mitaka
      yum -y install python-swiftclient
  SHELL

    config.vm.define "db1" do |db1|
      db1.vm.hostname = "db1.example.com"
      db1.vm.network "private_network", ip: "192.168.56.111"
      db1.vm.base_mac = "5CA1AB1E0210"
      db1.vm.provider "virtualbox" do |vb|
        vb.gui = true
        vb.memory = "1024"
        #vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata", "--controller", "IntelAHCI", "--portcount", 3]
        #vb.customize ["createmedium", "disk", "--filename", "./db1d1.vdi", "--size", 1024]
      end
      db1.vm.provision "shell", inline: <<-SHELL
        #yum -y update
        touch /etc/yum.repos.d/datastax.repo
        echo "[datastax]" >> /etc/yum.repos.d/datastax.repo
        echo "name = DataStax Repo for Apache Cassandra" >> /etc/yum.repos.d/datastax.repo
        echo "baseurl = http://rpm.datastax.com/community" >> /etc/yum.repos.d/datastax.repo
        echo "enabled = 1" >> /etc/yum.repos.d/datastax.repo
        echo "gpgcheck = 0" >> /etc/yum.repos.d/datastax.repo
        yum -y install java-1.8.0-openjdk dsc30 cassandra30-tools
        chkconfig cassandra on
        cp /tmp/vagrant/cassandra.yaml /etc/cassandra/conf/cassandra.yaml
        service cassandra start
        sleep 15
        cqlsh 192.168.56.111 -f /tmp/vagrant/mykeyspace.cql 
      SHELL
    end
#  (2..3).each do |i|
#    config.vm.define "db#{i}" do |node|
#      node.vm.hostname = "db#{i}.example.com"
#      node.vm.network "private_network", ip: "192.168.56.11#{i}"
#      node.vm.base_mac = "5CA1AB1E001#{i}"
#      node.vm.provider "virtualbox" do |vb|
#        vb.gui = true
#        vb.memory = "1024"
#        #vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata", "--controller", "IntelAHCI", "--portcount", 3]
#        #vb.customize ["createmedium", "disk", "--filename", "./db#{i}d1.vdi", "--size", 1024]
#        #vb.customize ["storageattach", :id, "--storagectl", "SATA", "--port", 0, "--device", 0, "--type", "hdd", "--medium", "./db#{i}d1.vdi"]
#      end
#      node.vm.provision "shell", inline: <<-SHELL
#        #yum -y update 
#        touch /etc/yum.repos.d/datastax.repo
#        echo "[datastax]" >> /etc/yum.repos.d/datastax.repo
#        echo "name = DataStax Repo for Apache Cassandra" >> /etc/yum.repos.d/datastax.repo
#        echo "baseurl = http://rpm.datastax.com/community" >> /etc/yum.repos.d/datastax.repo
#        echo "enabled = 1" >> /etc/yum.repos.d/datastax.repo
#        echo "gpgcheck = 0" >> /etc/yum.repos.d/datastax.repo
#        yum -y install java-1.8.0-openjdk dsc30 cassandra30-tools
#        chkconfig cassandra on
#        cp /tmp/vagrant/cassandra.yaml /etc/cassandra/conf/cassandra.yaml
#        service cassandra start
#      SHELL
#    end
    config.vm.define "mgmt" do |mgmt|
      mgmt.vm.hostname = "mgmt.example.com"
      mgmt.vm.network "private_network", ip: "192.168.56.101"
      mgmt.vm.base_mac = "5CA1AB1E0211"
      mgmt.vm.provider "virtualbox" do |vb|
        vb.gui = true
        vb.memory = "1024"
        #vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata", "--controller", "IntelAHCI", "--portcount", 3]
        #vb.customize ["createmedium", "disk", "--filename", "./db#{i}d1.vdi", "--size", 1024]
      end
      mgmt.vm.provision "shell", inline: <<-SHELL
        echo "[datastax]" >> /etc/yum.repos.d/datastax.repo
        echo "name = DataStax Repo for Apache Cassandra" >> /etc/yum.repos.d/datastax.repo
        echo "baseurl = http://rpm.datastax.com/community" >> /etc/yum.repos.d/datastax.repo
        echo "enabled = 1" >> /etc/yum.repos.d/datastax.repo
        echo "gpgcheck = 0" >> /etc/yum.repos.d/datastax.repo
        yum -y install java-1.8.0-openjdk cassandra30-tools
      SHELL
    end
#  end
end
