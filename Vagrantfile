VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "saucy64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/saucy/current/saucy-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.network :forwarded_port, guest: 80, host: 8888

  config.berkshelf.enabled = true
  config.omnibus.chef_version = :latest

  config.vm.synced_folder "/Projects", "/Projects"

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "4096"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  end

  config.vm.provision :chef_solo do |chef|
    chef.log_level = :info
    chef.add_recipe "java"
    chef.add_recipe "git"
    chef.add_recipe "nodejs"
    chef.add_recipe "ruby"
#    chef.add_recipe "nginx"
    chef.add_recipe "emacs"

    chef.json = { 
      :java => { 
        "install_flavor" => "oracle",
        "jdk_version" => "7",
        "oracle" => {
          "accept_oracle_download_terms" => true
        },
        "jdk" => {"7" => {
            "i586" => {"url" => "http://191.236.23.180/jdk-7u51-linux-i586.tar.gz"},
            "x86_64" => {"url" => "http://191.236.23.180/jdk-7u51-linux-x64.tar.gz" } } 
        }
      },
      :git => { :version => "1.7.9.5" },
      :nodejs => { :version => "0.11.12" },
      :languages => { :ruby => { :default_version => "1.8" } },
      :emacs => { "packages" => ["emacs24-nox"] }
  }
  end
end