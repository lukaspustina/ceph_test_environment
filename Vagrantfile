Vagrant.configure("2") do |config|
  VAGRANT_ROOT = File.dirname(File.expand_path(__FILE__))

  config.vm.box = "ubuntu/trusty64"

  config.vm.define "alpha" do |alpha|
    alpha.vm.network "private_network", ip:  "192.168.111.11"
    alpha.vm.host_name = "alpha.local"

    alpha.vm.provider "virtualbox" do | vm |
      file_to_disk = File.join(VAGRANT_ROOT, 'alpha-2.vdi')
      unless File.exist?(file_to_disk)
        vm.customize ['createhd', '--filename', file_to_disk, '--size', 500 * 1024]
      end
      vm.customize ['storageattach', :id, '--storagectl', 'SATAController', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
    end
  end

  config.vm.define "beta" do |beta|
    beta.vm.network "private_network", ip: "192.168.111.12"
    beta.vm.host_name = "beta.local"

    beta.vm.provider "virtualbox" do | vm |
      file_to_disk = File.join(VAGRANT_ROOT, 'beta-2.vdi')
      unless File.exist?(file_to_disk)
        vm.customize ['createhd', '--filename', file_to_disk, '--size', 500 * 1024]
      end
      vm.customize ['storageattach', :id, '--storagectl', 'SATAController', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
    end
  end

  config.vm.define "gamma" do |gamma|
    gamma.vm.network "private_network", ip: "192.168.111.13"
    gamma.vm.host_name = "gamma.local"

    gamma.vm.provider "virtualbox" do | vm |
      file_to_disk = File.join(VAGRANT_ROOT, 'gamma-2.vdi')
      unless File.exist?(file_to_disk)
        vm.customize ['createhd', '--filename', file_to_disk, '--size', 500 * 1024]
      end
      vm.customize ['storageattach', :id, '--storagectl', 'SATAController', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.groups = {
      "mons" => ["alpha", "beta", "gamma"],
      "osds" => ["alpha", "beta", "gamma"],
      "all:children" => ["mons", "osds"]
    }
    ansible.playbook = "site.yml"
  end

end



