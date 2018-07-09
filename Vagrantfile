Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.ssh.insert_key = false

  def create_host(config, hostname, ip, memory, cpus)
    config.vm.define hostname do |machine|
      machine.vm.hostname = hostname
      machine.vm.network "private_network", ip: ip
      machine.vm.provider "virtualbox" do |vb|
        vb.memory = memory
        vb.cpus = cpus
      end#provider
      yield machine if block_given?
    end#define
  end#def

  create_host(config, "wp01", "192.168.99.100", 512, 1)
  create_host(config, "app01", "192.168.99.101", 512, 1)
  create_host(config, "app02", "192.168.99.102", 512, 1) do |host|
    host.vm.provision "ansible" do |ansible|
      ansible.limit = "all"
      ansible.inventory_path = "./hosts"
      ansible.playbook = "playbook.yml"
    end#provision
  end#create_host

end
