IMAGE_NAME = "generic/ubuntu2004"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
    config.proxy.http     = "http://192.168.50.1:9010"
    config.proxy.https     = "http://192.168.50.1:9010"
    config.proxy.no_proxy = "localhost,127.0.0.1"

    config.vm.provider "libvirt" do |v|
        v.memory = 8192
        v.cpus = 8
    end

    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "192.168.50.10"
        master.vm.hostname = "k8s-master"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "kubernetes-setup/master-playbook.yml"
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
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "192.168.50.#{i + 10}",
                }
            end
        end
    end
end
