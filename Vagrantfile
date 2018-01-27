Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

# avoid self signed SSL issues
  config.vm.box_download_insecure = true
  config.vm.network "forwarded_port", guest:80, host:8080, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest:3306, host:3306, host_ip: "127.0.0.1"

  config.vm.synced_folder ".", "/vagrant",
    :mount_options => ["dmode=777", "fmode=777"]

  # optional
  config.vm.provider "virtualbox" do |v|
    v.memory=2048
    v.cpus=1
  end

end
