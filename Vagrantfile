# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  # Set box configuration
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.customize ["modifyvm", :id, "--memory", 2048]
  
  # Assign this VM to a host-only network IP, allowing you to access it via the IP.
  config.vm.network :hostonly, "33.33.33.10"

  # Enable provisioning with chef solo, specifying a cookbooks path (relative
  # to this Vagrantfile), and adding some recipes and/or roles.
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "cookbooks"
    chef.data_bags_path = "data_bags"
    chef.add_recipe "vagrant_main"

    chef.json.merge!({
      "mysql" => {
        "server_root_password" => "vagrant",
        "bind_address" => "0.0.0.0"
      }
    })
  end
  
  config.vm.share_folder "v-root", "/vagrant", ".", :extra => 'dmode=775,fmode=664', :nfs => true
  # /var/log over NFS will not work, probably something to do with trying to mount something on a location that already exists.
  config.vm.share_folder "v-logs", "/var/log", "vagrant-logs", :owner=> 'vagrant', :group=>'www-data', :extra => 'dmode=775,fmode=664'
end
