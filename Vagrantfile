Vagrant.configure(2) do |config|
  config.vm.box = "puppetlabs/centos-7.0-64-nocm"
  config.vm.provision "shell", path: "bootstrap.sh"
  config.vm.provision "shell", path: "build.sh"
end
