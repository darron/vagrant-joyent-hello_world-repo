# hello joyent

$smartos_chef = <<SCRIPT

getent passwd | grep vagrant
if [ $? -gt 0 ]; then
  useradd vagrant
fi

pkgin list | grep ^emacs-nox11-[0-9]
if [ $? -gt 0 ]; then
  pkgin -y install emacs-nox11
fi ;

# ruby
pkgin list | grep ^ruby193-[0-9] ;
if [ $? -gt 0 ]; then 
  pkgin -y install ruby193 ;
fi ;
    
# gmake
pkgin list | grep ^gmake-[0-9] ;
if [ $? -gt 0 ]; then
  pkgin -y install gmake ;
fi ;
      
# gcc
pkgin list | grep ^gcc47-[0-9] ;
if [ $? -gt 0 ]; then
  pkgin -y install gcc47 ;
fi ;

# chef
gem list | grep ^chef
if [ $? -gt 0 ]; then  
  gem install chef --no-ri -no-rdoc ;
fi
  
# chef-client symlink
test -h /opt/local/bin/chef-client ;
if [ $? -gt 0 ]; then  
  ln -s /opt/local/lib/ruby/gems/1.9.3/gems/chef-11.4.0/bin/chef-client /opt/local/bin/chef-client ;
fi

# chef-client symlink
test -h /opt/local/bin/chef-solo ;
if [ $? -gt 0 ]; then  
  ln -s /opt/local/lib/ruby/gems/1.9.3/gems/chef-11.4.0/bin/chef-solo /opt/local/bin/chef-solo ;
fi
SCRIPT


  Vagrant.configure("2") do |config|
    config.vm.box = "dummy"
  
    config.vm.provider :joyent do |joyent|
      # Joyent login
      joyent.joyent_username = ENV['JOYENT_USERNAME']
      joyent.joyent_password = ENV['JOYENT_PASSWORD']
      joyent.joyent_api_url  = ENV['JOYENT_API_URL']
      
      # base64 1.9.0
      joyent.dataset = "bad2face-8738-11e2-ac72-0378d02f84de"
      
      joyent.flavor = "Small 1GB"
      joyent.node_name = "vagrant-test"
      joyent.ssh_username = "root"
      joyent.ssh_private_key_path = ENV['JOYENT_SSH_PRIVATE_KEY_PATH'] 
    end

    # bootstrap chef
    config.vm.provision :shell, :inline => $smartos_chef
    
    # apply a run_list
    config.vm.provision :chef_solo do |chef|
      chef.run_list = [
        "recipe[vagrant-joyent-hello_world::default]"]
    end
    
  end
  
