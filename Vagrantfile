# Runs the role against CentOS 7
# for local testing purposes
# Requires vagrant-libvirt

Vagrant.configure("2") do |config|
  config.vm.provider :libvirt do |libvirt|
    libvirt.random :model => 'random'
  end
  config.vm.synced_folder ".", "/vagrant", type: "nfs"
  config.vm.define "centos7" do |centos7|
    centos7.vm.box = "centos/7"
    centos7.vm.hostname = "tpm2-simulator"
    centos7.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
    end
  end
end
