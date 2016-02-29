Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/wily64"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--groups", "/hadoop-cluster"]
  end

  # vagrant-cachier speeds up provisioning by caching downloaded Linux packages.
  # To install, run `vagrant plugin install vagrant-cachier`.
  # See http://fgrehm.viewdocs.io/vagrant-cachier/.
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.provision "shell", path: "provision.sh"

  config.vm.define "master" do |c|
    c.vm.hostname = "hadoop-master"
    c.vm.network "private_network", ip: "192.168.50.4"
    c.vm.provider "virtualbox" do |vb|
      vb.name = "hadoop-master"
      vb.cpus = 2
      vb.memory = "2048"
    end
    # Save master node's public key
    c.vm.provision "shell", inline: """
      mkdir -p /vagrant/tmp
      cat /home/vagrant/.ssh/id_rsa.pub > /vagrant/tmp/master_public_key
    """
  end

  (1..2).each do |i|
    config.vm.define "slave-#{i}" do |c|
      c.vm.hostname = "hadoop-slave-#{i}"
      c.vm.network "private_network", ip: "192.168.50.#{4 + i}"
      c.vm.provider "virtualbox" do |vb|
        vb.name = "hadoop-slave-#{i}"
        vb.cpus = 2
        vb.memory = "4096"
      end
      # Authorise passphraseless SSH from master to slaves
      c.vm.provision "shell", inline: """
        cat /vagrant/tmp/master_public_key >> /home/vagrant/.ssh/authorized_keys
      """
    end
  end
end
