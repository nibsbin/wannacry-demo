# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.synced_folder '.', '/vagrant', disabled: true

  config.vm.define "kali" do |kali|
    kali.vm.box = "kalilinux/rolling"
    kali.vm.hostname = "kali"

    kali.vm.network "private_network", ip: '192.168.24.2', name: "VirtualBox Host-Only Ethernet Adapter #2"

    kali.vm.provision "shell", run: "always", inline: <<-SHELL
      nmcli con show "eth1-static" &>/dev/null || \
        nmcli con add type ethernet ifname eth1 con-name "eth1-static" \
          ipv4.addresses 192.168.24.2/24 ipv4.method manual
      nmcli con up "eth1-static" || true
    SHELL

    kali.vm.provider "virtualbox" do |v|
      v.name = "kali"
      v.memory = 4096
      v.cpus = 2
    end

    kali.vm.provision "file",
      source: "./memo_maker.exe",
      destination: "/home/vagrant/memo_maker.exe"
  end

  config.vm.define "win2k8_1" do |win|
    win.vm.box = "rapid7/metasploitable3-win2k8"
    win.vm.hostname = "Lt-Connolys-PC"
    win.vm.communicator = "winrm"
    win.vm.boot_timeout = 600
    win.vm.graceful_halt_timeout = 120
    win.winrm.transport = :negotiate
    win.winrm.basic_auth_only = false
    win.winrm.retry_limit = 90
    win.winrm.retry_delay = 10

    win.vm.network "private_network", ip: '192.168.24.3', name: "VirtualBox Host-Only Ethernet Adapter #2"

    win.vm.provider "virtualbox" do |v|
      v.name = "Lt Connoly's PC"
      v.memory = 4096
      v.cpus = 2
    end

    # Configure Firewall to open up vulnerable services
    case ENV['MS3_DIFFICULTY']
      when 'easy'
        win.vm.provision :shell, inline: "C:\\startup\\disable_firewall.bat"
      else
        win.vm.provision :shell, inline: "C:\\startup\\enable_firewall.bat"
        win.vm.provision :shell, inline: "C:\\startup\\configure_firewall.bat"
    end

    win.vm.provision :shell, inline: "C:\\startup\\install_share_autorun.bat"
    win.vm.provision :shell, inline: "C:\\startup\\setup_linux_share.bat"
    win.vm.provision :shell, inline: "Remove-Item C:\\startup\\* -Force"
  end

  config.vm.define "win2k8_2" do |win|
    win.vm.box = "rapid7/metasploitable3-win2k8"
    win.vm.hostname = "Capt-Dorfners-PC"
    win.vm.communicator = "winrm"
    win.vm.boot_timeout = 600
    win.vm.graceful_halt_timeout = 120
    win.winrm.transport = :negotiate
    win.winrm.basic_auth_only = false
    win.winrm.retry_limit = 90
    win.winrm.retry_delay = 10

    win.vm.network "private_network", ip: '192.168.24.4', name: "VirtualBox Host-Only Ethernet Adapter #2"

    win.vm.provider "virtualbox" do |v|
      v.name = "Capt Dorfner's PC"
      v.memory = 4096
      v.cpus = 2
    end

    # Configure Firewall to open up vulnerable services
    case ENV['MS3_DIFFICULTY']
      when 'easy'
        win.vm.provision :shell, inline: "C:\\startup\\disable_firewall.bat"
      else
        win.vm.provision :shell, inline: "C:\\startup\\enable_firewall.bat"
        win.vm.provision :shell, inline: "C:\\startup\\configure_firewall.bat"
    end

    win.vm.provision :shell, inline: "C:\\startup\\install_share_autorun.bat"
    win.vm.provision :shell, inline: "C:\\startup\\setup_linux_share.bat"
    win.vm.provision :shell, inline: "Remove-Item C:\\startup\\* -Force"
  end

  config.vm.define "monitor" do |monitor|
    monitor.vm.box = "kalilinux/rolling"
    monitor.vm.hostname = "monitor"

    monitor.vm.network "private_network", ip: '192.168.24.5', name: "VirtualBox Host-Only Ethernet Adapter #2"

    monitor.vm.provision "shell", run: "always", inline: <<-SHELL
      nmcli con show "eth1-static" &>/dev/null || \
        nmcli con add type ethernet ifname eth1 con-name "eth1-static" \
          ipv4.addresses 192.168.24.5/24 ipv4.method manual
      nmcli con up "eth1-static" || true
    SHELL

    monitor.vm.provider "virtualbox" do |v|
      v.name = "monitor"
      v.memory = 4096
      v.cpus = 2
    end
  end
end
