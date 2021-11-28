IMAGE_NAME = "bento/ubuntu-20.04"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
    config.vm.synced_folder ".", "/vagrant", disabled: false
    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
        v.gui = false
    end
      
    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "192.168.50.10"
        master.vm.hostname = "k8s-master"
        master.vm.provision "shell", inline: <<-SHELL
          sudo apt-get updates
          sudo apt-get install -y ansible
          sudo mkdir -p /vagrant
        SHELL
        master.vm.provision "file", source: "kubernetes-setup/master-playbook.yml", destination: "/tmp/master-playbook.yml"
        master.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "/tmp/master-playbook.yml"
            ansible.extra_vars = {
                node_ip: "192.168.50.10",
            }
        end
    end

    (1..N).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
            node.vm.hostname = "node-#{i}"
            node.vm.provision "shell", inline: <<-SHELL
              sudo apt-get update
              sudo apt-get install -y ansible
              sudo mkdir -p /vagrant
            SHELL
            node.vm.provision "file", source: "kubernetes-setup/node-playbook.yml", destination: "/tmp/node-playbook.yml"
            node.vm.provision "file", source: "join-command", destination: "/tmp/join-command"
            node.vm.provision "ansible_local" do |ansible|
                ansible.playbook = "/tmp/node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "192.168.50.#{i + 10}",
                }
            end
        end
    end
end
