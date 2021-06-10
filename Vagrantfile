Vagrant.configure("2") do |config|

  config.vm.define :master do |master|
    master.vm.provider :virtualbox do |vb|
      vb.name = "master"
      vb.memory = 2048
      vb.cpus = 2
    end
    master.vm.box = "bento/ubuntu-18.04"
    master.vm.hostname = "master"
    master.vm.network :private_network, ip: "192.168.56.80"
  end

  %w{node1 node2}.each_with_index do |name, i|
    config.vm.define name do |node|
      node.vm.provider "virtualbox" do |vb|
        vb.name = "node#{i + 1}"
        vb.memory = 1024
        vb.cpus = 1
      end
      node.vm.box = "bento/ubuntu-18.04"
      node.vm.hostname = name
      node.vm.network :private_network, ip: "192.168.56.#{i + 81}"
    end
  end
end