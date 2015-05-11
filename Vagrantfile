# encoding: utf-8
# This file originally created at http://rove.io/270c79887938c5a281d65863ddf38085

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box      = 'ubuntu/trusty32'
  config.vm.hostname = 'rails-dev-box'
  config.ssh.forward_agent = true

  config.vm.network :forwarded_port, guest: 3000, host: 3000

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks"]
    chef.add_recipe :apt
    chef.add_recipe 'postgresql::server'
    chef.add_recipe 'git'
    chef.add_recipe 'nodejs'
    chef.add_recipe "ruby_build"
    chef.add_recipe "rbenv::user"
    chef.add_recipe "rbenv::vagrant"
    chef.add_recipe 'vim'
    chef.add_recipe 'redis'
    chef.add_recipe 'python'
    chef.json = {
      rbenv: {
        user_installs: [{
          user: 'vagrant',
          rubies: ["2.2.2"],
          global: "2.2.2",
          gems:   {
            '2.2.2' => [{'name'    => 'bundler'}]
          }
        }]
      },
      :postgresql => {
        :config   => {
          :listen_addresses => "*",
          :port             => "5432"
        },
        :pg_hba   => [
          {
            :type   => "local",
            :db     => "postgres",
            :user   => "postgres",
            :addr   => nil,
            :method => "trust"
          },
          {
            :type   => "host",
            :db     => "all",
            :user   => "all",
            :addr   => "0.0.0.0/0",
            :method => "md5"
          },
          {
            :type   => "host",
            :db     => "all",
            :user   => "all",
            :addr   => "::1/0",
            :method => "md5"
          }
        ],
        :password => {
          :postgres => "password"
        }
      },
      :git        => {
        :prefix => "/usr/local"
      },
      :vim        => {
        :extra_packages => [
          "vim-rails"
        ]
      },
      :redis      => {
        :bind        => "127.0.0.1",
        :port        => "6379",
        :config_path => "/etc/redis/redis.conf",
        :daemonize   => "yes",
        :timeout     => "300",
        :loglevel    => "notice"
      }
    }
  end
end
