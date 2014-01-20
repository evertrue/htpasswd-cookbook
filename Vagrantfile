# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  config.vm.hostname = "htpasswd-berkshelf"

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise64"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "http://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-vagrant-amd64-disk1.box"

  config.vm.provision :shell, :inline => "curl -s -L https://www.opscode.com/chef/install.sh | sudo bash"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.synced_folder "#{ENV['HOME']}/.chef", "/tmp/local-chef"

  if ENV['CHEF_REPO']
    chef_repo = ENV['CHEF_REPO']
  else
    raise "CHEF_REPO is not defined"
  end

  config.chef_zero.chef_repo_path = ".chef-zero/"

  config.vm.network :private_network, ip: "33.33.33.10"
  config.berkshelf.enabled = true

  config.vm.provision :chef_solo do |chef|
    chef.json = {}

    chef.data_bags_path = "#{chef_repo}/data_bags"
    chef.encrypted_data_bag_secret_key_path = "#{ENV['HOME']}/.chef/encrypted_data_bag_secret"
    chef.node_name = "et-base-berkshelf"
    chef.log_level = :info

    chef.run_list = [
        "recipe[htpasswd::default]"
    ]
  end
end
