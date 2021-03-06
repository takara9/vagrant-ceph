# coding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  ## Cephノード 仮想マシンの起動 #1
  #
  config.vm.define 'node1' do |machine|
    machine.vm.box = "ubuntu/bionic64"
    machine.vm.hostname = 'node1'
    machine.vm.network :private_network,ip: "172.16.1.31"
    #machine.vm.network :public_network, ip: "192.168.1.91", bridge: "en0: Ethernet"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false        
      vbox.cpus = 1
      vbox.memory = 1536
      # DISK
      vdisk = "vdisk/sdb-1.vdi"
      # CREATE DISK
      if not File.exist?(vdisk) then
         vbox.customize [
           'createmedium', 'disk',
           '--filename', vdisk,
           '--format', 'VDI',
           '--size', 102400 ]
           # 1024 * 100 = 100GB 
      end
      # ATTACH DISK
      vbox.customize [
        'storageattach', :id,
        '--storagectl', 'SCSI',
        '--port', 2,
        '--device', 0,
        '--type', 'hdd',
        '--medium', vdisk]
    end
    machine.vm.synced_folder ".", "/vagrant", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=700", "fmode=700"]

    machine.vm.provision "shell", inline: <<-SHELL
sudo sed -i.bak -e "s%http://us.archive.ubuntu.com/ubuntu/%http://ftp.iij.ad.jp/pub/linux/ubuntu/archive/%g" /etc/apt/sources.list
SHELL

    ## deploy-cephでリモート操作する為の設定
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook       = "playbook/install.yaml"
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = true
      ansible.limit          = "node1"
      ansible.inventory_path = "hosts_vagrant"      
    end
  end


  ## Cephノード 仮想マシンの起動 #2
  #
  config.vm.define 'node2' do |machine|
    machine.vm.box = "ubuntu/bionic64"
    machine.vm.hostname = 'node2'
    machine.vm.network :private_network,ip: "172.16.1.32"
    #machine.vm.network :public_network, ip: "192.168.1.92", bridge: "en0: Ethernet"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false        
      vbox.cpus = 1
      vbox.memory = 1536
      # DISK
      vdisk = "vdisk/sdb-2.vdi"
      # CREATE DISK
      if not File.exist?(vdisk) then
         vbox.customize [
           'createmedium', 'disk',
           '--filename', vdisk,
           '--format', 'VDI',
           '--size', 102400 ]
           # 1024 * 100 = 100GB 
      end
      # ATTACH DISK
      vbox.customize [
        'storageattach', :id,
        '--storagectl', 'SCSI',
        '--port', 2,
        '--device', 0,
        '--type', 'hdd',
        '--medium', vdisk]
    end
    machine.vm.synced_folder ".", "/vagrant", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=700", "fmode=700"]

    machine.vm.provision "shell", inline: <<-SHELL
sudo sed -i.bak -e "s%http://us.archive.ubuntu.com/ubuntu/%http://ftp.iij.ad.jp/pub/linux/ubuntu/archive/%g" /etc/apt/sources.list
SHELL

    ## deploy-cephでリモート操作する為の設定
    #
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook       = "playbook/install.yaml"      
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = true
      ansible.limit          = "node2"
      ansible.inventory_path = "hosts_vagrant"
    end
  end

  ## Cephノード 仮想マシンの起動 #3
  #
  config.vm.define 'node3' do |machine|
    machine.vm.box = "ubuntu/bionic64"
    machine.vm.hostname = 'node3'
    machine.vm.network :private_network,ip: "172.16.1.33"
    #machine.vm.network :public_network, ip: "192.168.1.93", bridge: "en0: Ethernet"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false        
      vbox.cpus = 1
      vbox.memory = 1536
      # DISK
      vdisk = "vdisk/sdb-3.vdi"
      # CREATE DISK
      if not File.exist?(vdisk) then
         vbox.customize [
           'createmedium', 'disk',
           '--filename', vdisk,
           '--format', 'VDI',
           '--size', 102400 ]
           # 1024 * 100 = 100GB 
      end
      # ATTACH DISK
      vbox.customize [
        'storageattach', :id,
        '--storagectl', 'SCSI',
        '--port', 2,
        '--device', 0,
        '--type', 'hdd',
        '--medium', vdisk]
    end
    machine.vm.synced_folder ".", "/vagrant", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=700", "fmode=700"]

    machine.vm.provision "shell", inline: <<-SHELL
sudo sed -i.bak -e "s%http://us.archive.ubuntu.com/ubuntu/%http://ftp.iij.ad.jp/pub/linux/ubuntu/archive/%g" /etc/apt/sources.list
SHELL

    ## ノード間連携のセットアップ
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook       = "playbook/install.yaml"      
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = true
      ansible.limit          = "node3"
      ansible.inventory_path = "hosts_vagrant"
    end
  end


  
  ## Ceph管理ノード
  #
  config.vm.define 'master' do |machine|
    machine.vm.box = "ubuntu/bionic64"
    machine.vm.hostname = 'master'
    machine.vm.network :private_network,ip: "172.16.1.30"
    machine.vm.network :public_network, ip: "192.168.1.90", bridge: "en0: Ethernet"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false        
      vbox.cpus = 1
      vbox.memory = 1024
    end

    machine.vm.synced_folder ".", "/vagrant", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=700", "fmode=700"]

    machine.vm.provision "shell", inline: <<-SHELL
sudo sed -i.bak -e "s%http://us.archive.ubuntu.com/ubuntu/%http://ftp.iij.ad.jp/pub/linux/ubuntu/archive/%g" /etc/apt/sources.list
SHELL
    
    ## ceph-deployインストール他
    #
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook       = "playbook/install.yaml"      
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = true
      ansible.limit          = "master"
      ansible.inventory_path = "hosts_vagrant"
    end
  end


  
  ## Cephクライアントノード
  #
  config.vm.define 'client' do |machine|
    machine.vm.box = "ubuntu/bionic64"
    machine.vm.hostname = 'client'
    machine.vm.network :private_network,ip: "172.16.1.40"
    #machine.vm.network :public_network, ip: "192.168.1.229", bridge: "en0: Ethernet"
    machine.vm.provider "virtualbox" do |vbox|
      vbox.gui = false        
      vbox.cpus = 1
      vbox.memory = 1024
    end

    machine.vm.synced_folder ".", "/vagrant", owner: "vagrant",
      group: "vagrant", mount_options: ["dmode=700", "fmode=700"]

    machine.vm.provision "shell", inline: <<-SHELL
sudo sed -i.bak -e "s%http://us.archive.ubuntu.com/ubuntu/%http://ftp.iij.ad.jp/pub/linux/ubuntu/archive/%g" /etc/apt/sources.list
SHELL
    
    ## deploy-cephでリモート操作する為の設定
    #
    machine.vm.provision "ansible_local" do |ansible|
      ansible.playbook       = "playbook/install.yaml"      
      ansible.version        = "latest"
      ansible.verbose        = false
      ansible.install        = true
      ansible.limit          = "client"
      ansible.inventory_path = "hosts_vagrant"
    end
  end
end
