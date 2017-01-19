# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# We use this provisioner to write the vbox_host.cfg ansible inventory file,
# which makes it easier to use ansible-playbook directly.
module AnsibleInventory
    class Config < Vagrant.plugin("2", :config)
        attr_accessor :machine
    end

    class Plugin < Vagrant.plugin("2")
        name "write_vbox_cfg"

        config(:write_vbox_cfg, :provisioner) do
            Config
        end

        provisioner(:write_vbox_cfg) do
            Provisioner
        end
    end

    class Provisioner < Vagrant.plugin("2", :provisioner)
        def provision
          # get the output ov vagrant ssh-config <machine>
          require 'open3'
          stdin, stdout, stderr, wait_thr = Open3.popen3('vagrant', 'ssh-config', config.machine)
          output = stdout.gets(nil)
          stdout.close
          stderr.gets(nil)
          stderr.close
          exit_code = wait_thr.value.exitstatus
          if exit_code == 0
            # parse out the key variables
            /HostName (?<host>.+)/ =~ output
            /Port (?<port>.+)/ =~ output
            /User (?<user>.+)/ =~ output
            /IdentityFile (?<keyfile>.+)/ =~ output
            # write an ansible inventory file
            contents = "myhost ansible_ssh_port=#{port} ansible_ssh_host=#{host} ansible_ssh_user=#{user} ansible_ssh_private_key_file=#{keyfile} ansible_ssh_extra_args='-o StrictHostKeyChecking=no'\n"
            File.open("vbox_host.cfg", "w") do |aFile|
              aFile.puts(contents)
            end
          end
          result = exit_code
        end
    end
end


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 1
  end

  config.vm.define "wheezy", autostart: false do |wheezy|
    wheezy.vm.box = "debian/wheezy64"
    wheezy.vm.synced_folder ".", "/vagrant", disabled: true
    wheezy.vm.network "private_network", ip: "192.168.33.1"
    wheezy.vm.provision "write_vbox_cfg", machine: "wheezy"
    wheezy.vm.provision "ansible" do |ansible|
      ansible.playbook = "test.yml"
    end
  end

  config.vm.define "jessie", autostart: false do |jessie|
    jessie.vm.box = "debian/jessie64"
    jessie.vm.synced_folder ".", "/vagrant", disabled: true
    jessie.vm.network "private_network", ip: "192.168.33.2"
    jessie.vm.provision "write_vbox_cfg", machine: "jessie"
    jessie.vm.provision "ansible", playbook:"test.yml"
  end

  config.vm.define "precise", autostart: false do |precise|
    precise.vm.box = "bento/ubuntu-12.04"
    precise.vm.synced_folder ".", "/vagrant", disabled: true
    precise.vm.network "private_network", ip: "192.168.33.3"
    precise.vm.provision "write_vbox_cfg", machine: "precise"
    precise.vm.provision "ansible" do |ansible|
      ansible.playbook = "test.yml"
    end
  end

  config.vm.define "trusty", primary: false, autostart: false do |trusty|
    trusty.vm.box = "ubuntu/trusty64"
    trusty.vm.synced_folder ".", "/vagrant", disabled: true
    trusty.vm.network "private_network", ip: "192.168.33.4"
    trusty.vm.provision "write_vbox_cfg", machine: "trusty"
    trusty.vm.provision "ansible" do |ansible|
      ansible.playbook = "test.yml"
    end
  end

  config.vm.define "wily", autostart: false do |wily|
    wily.vm.box = "ubuntu/wily64"
    wily.vm.synced_folder ".", "/vagrant", disabled: true
    wily.vm.network "private_network", ip: "192.168.33.5"
    wily.vm.provision "write_vbox_cfg", machine: "wily"
    wily.vm.provision "ansible" do |ansible|
      ansible.playbook = "test.yml"
    end
  end

  config.vm.define "xenial", primary: true, autostart: true do |myhost|
    myhost.vm.box = "ubuntu/xenial64"
    myhost.vm.network "private_network", ip: "192.168.33.6"
    myhost.vm.provision "shell", inline: "apt-get install -y python"
    myhost.vm.provision "write_vbox_cfg", machine: "xenial"
    myhost.vm.provision "ansible" do |ansible|
      ansible.playbook = "test.yml"
    end
  end

  config.vm.define "centos6", primary: false, autostart: false do |centos6|
    centos6.vm.box = "bento/centos-6.7"
    centos6.vm.synced_folder ".", "/vagrant", disabled: true
    centos6.vm.network "private_network", ip: "192.168.33.7"
    centos6.vm.provision "write_vbox_cfg", machine: "centos6"
    centos6.vm.provision "ansible" do |ansible|
      ansible.playbook = "test.yml"
    end
  end

  config.vm.define "centos7", primary: false, autostart: false do |centos7|
    centos7.vm.box = "centos/7"
    centos7.vm.synced_folder ".", "/vagrant", disabled: true
    centos7.vm.network "private_network", ip: "192.168.33.8"
    centos7.vm.provision "write_vbox_cfg", machine: "centos7"
    centos7.vm.provision "ansible" do |ansible|
      ansible.playbook = "test.yml"
    end
  end

end
