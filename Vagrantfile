# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  REDISSERVERS = 3
  PROXIES = 2

  EXTERNAL_IPs = ['192.168.88.100',
                  '192.168.88.101',
                  '192.168.88.102']

  (1..PROXIES).each do |machine_id|
    config.vm.define "haproxy#{machine_id}" do |haproxy|

      haproxy.vm.box = "ubuntu/trusty64"
      haproxy.vm.hostname = "haproxy#{machine_id}"
      haproxy.vm.box_check_update = false
      haproxy.vm.network "private_network", type: "dhcp"
      haproxy.vm.network "public_network", bridge: "wlan0", ip: EXTERNAL_IPs[machine_id-1]
      haproxy.vm.provider "virtualbox" do |v|
        v.memory = 256
        v.cpus = 1
      end
    end
  end

  (1..REDISSERVERS).each do |machine_id|
    config.vm.define "redis#{machine_id}" do |redis|
      redis.vm.box = "ubuntu/trusty64"
      redis.vm.hostname = "redis#{machine_id}"
      redis.vm.box_check_update = false
      redis.vm.network "private_network", type: "dhcp"
      redis.vm.network "public_network", bridge: "wlan0", ip: "192.168.88.#{20+machine_id-1}"
      redis.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 1
      end

      if machine_id == REDISSERVERS
        redis.vm.provision :ansible do |ansible|
          ansible.playbook = "services/redis_cluster_full_install.yml"
          ansible.groups = {
              "haproxy" => ["haproxy[1:#{PROXIES}]"],
              "redis" => ["redis[1:#{REDISSERVERS}]"]
          }
        end
      end

    end
  end
end
