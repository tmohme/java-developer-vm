
Vagrant::configure("2") do |config|

  # configure the basebox
  config.vm.box = "tknerr/ubuntu1604-desktop"
  config.vm.box_version = "2.0.27.1"

  # override the basebox when testing (an approximation) with docker
  config.vm.provider :docker do |docker, override|
    override.vm.box = "tknerr/baseimage-ubuntu-16.04"
    override.vm.box_version = "1.0.0"
  end

  # virtualbox customizations
  config.vm.provider :virtualbox do |vbox, override|
    vbox.customize ["modifyvm", :id,
      "--name", "Java Developer VM",
      "--memory", 4096,
      "--cpus", 4
    ]
    vbox.gui = true
  end

  # set the hostname
  config.vm.hostname = "java-developer-vm.local"
  # don't create a new keypair
  config.ssh.insert_key = false

  # Install ChefDK and trigger the Chef run from within the VM
  config.vm.provision "shell", privileged: false, keep_color: true, run: 'always',
    inline: "/vagrant/scripts/update-vm.sh #{ENV['UPDATE_VM_FLAGS']}"

  # Ensure we cache as much as possible
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.enable :generic, {
      "chef_file_cache" => { cache_dir: "/root/.chef/local-mode-cache/cache" },
      "berks_cache" => { cache_dir: "/home/vagrant/.berkshelf" },
      "m2_repo" => { cache_dir: "/home/vagrant/.m2/repository" }
    }
  end
end
