# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Set box configuration
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", 2048]
  end

  # Assign this VM to a host-only network IP, allowing you to access it via the IP.
  config.vm.network :private_network, ip: "33.33.33.10"

  # Enable provisioning with chef solo, specifying a cookbooks path (relative
  # to this Vagrantfile), and adding some recipes and/or roles.
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "cookbooks"
    chef.data_bags_path = "data_bags"
    # chef.log_level = :debug
    chef.add_recipe "vagrant_main"

    chef.json.merge!({
      "mysql" => {
        "server_root_password" => "vagrant",
        "bind_address" => "0.0.0.0"
      }
    })
  end

  config.vm.synced_folder "./", "/vagrant/", id: "vagrant-root", :nfs => true
  # /var/log over NFS will not work, probably something to do with trying to mount something on a location that already exists.
  config.vm.synced_folder "vagrant-logs/", "/var/log/", id: "vagrant-logs", :owner=> 'vagrant', :group=>'www-data', :mount_options => ["dmode=775", "fmode=664"]
end
