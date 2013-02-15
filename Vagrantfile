# -*- mode: ruby -*-
# vi: set ft=ruby :
  require 'rubygems'
  require 'bundler'

  Bundler.require
  require 'multi_json'

Vagrant::Config.run do |config|
  config.vm.define :redis do |redis_config|
    redis_config.vm.box = "precise64"
    redis_config.vm.customize ["modifyvm", :id, "--name", "Redis"]
    redis_config.vm.forward_port 6379, 6379

    VAGRANT_JSON = MultiJson.load(Pathname(__FILE__).dirname.join('nodes', 'redis.json').read)

    redis_config.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = ["site-cookbooks", "cookbooks"]
      chef.roles_path = "roles"
      chef.data_bags_path = "data_bags"
      chef.provisioning_path = "/tmp/vagrant-chef"

      chef.json = VAGRANT_JSON
      VAGRANT_JSON['run_list'].each do |recipe|
        chef.add_recipe(recipe)
      end if VAGRANT_JSON['run_list']
    end
  end
  config.vm.define :rabbit do |rabbit_config|
    rabbit_config.vm.box = "precise64"
    rabbit_config.vm.customize ["modifyvm", :id, "--name", "RabbitMQ"]
 
    rabbit_config.vm.forward_port 5672, 5672
    rabbit_config.vm.forward_port 15672, 15672
   
    VAGRANT_JSON = MultiJson.load(Pathname(__FILE__).dirname.join('nodes', 'rabbit.json').read)

    rabbit_config.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = ["site-cookbooks", "cookbooks"]
      chef.roles_path = "roles"
      chef.data_bags_path = "data_bags"
      chef.provisioning_path = "/tmp/vagrant-chef"

      chef.json = VAGRANT_JSON
      VAGRANT_JSON['run_list'].each do |recipe|
        chef.add_recipe(recipe)
      end if VAGRANT_JSON['run_list']
    end
  end

  config.vm.define :db do |db_config|
    db_config.vm.box = "precise64"
    db_config.vm.customize ["modifyvm", :id, "--name", "Postgresql"]

    db_config.vm.forward_port 5432, 5432

    VAGRANT_JSON = MultiJson.load(Pathname(__FILE__).dirname.join('nodes', 'postgresql.json').read)

    db_config.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = ["site-cookbooks", "cookbooks"]
      chef.roles_path = "roles"
      chef.data_bags_path = "data_bags"
      chef.provisioning_path = "/tmp/vagrant-chef"

      chef.json = VAGRANT_JSON
      VAGRANT_JSON['run_list'].each do |recipe|
        chef.add_recipe(recipe)
      end if VAGRANT_JSON['run_list']
    end
  end
  config.vm.define :single do |single_config|
    single_config.vm.box = "precise64"
    single_config.vm.customize ["modifyvm", :id, "--name", "Single"]

    single_config.vm.forward_port 5432, 5432
    single_config.vm.forward_port 5672, 5672
    single_config.vm.forward_port 15672, 15672
    single_config.vm.forward_port 6379, 6379

    VAGRANT_JSON = MultiJson.load(Pathname(__FILE__).dirname.join('nodes', 'single.json').read)

    single_config.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = ["site-cookbooks", "cookbooks"]
      chef.roles_path = "roles"
      chef.data_bags_path = "data_bags"
      chef.provisioning_path = "/tmp/vagrant-chef"

      chef.json = VAGRANT_JSON
      VAGRANT_JSON['run_list'].each do |recipe|
        chef.add_recipe(recipe)
      end if VAGRANT_JSON['run_list']
    end
  end
end
