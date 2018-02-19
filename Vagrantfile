VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    
	## 'www' is an arbitrary identifier for this machine within this file.
	## This is useful when you want the same Vagrant file to control 
	## multiple machines, say when spinning up a cluster of servers.	
	config.vm.define "www" do |www|
		
		## Use Ubuntu 14.04 for the VM's OS. Vagrant will ony download this once. 
		## If anther project uses the same OS, Vagrant will use a cached version.	   
		www.vm.box = "ubuntu/trusty64"
		
		## If you're using the vagrant-hostsupdater plugin, this will also get added
		## to your hosts file
		www.vm.hostname = "bealers-24ways.dev"
        
		## Example of configuring the VM's 'hardware' on-the-fly.
		## Give the VM 2 virtual CPUs
    ## Note: this is specific for VirtualBox, but you could also hav a secion for WMWare
		www.vm.provider :virtualbox do |vb|
			vb.customize ["modifyvm", :id, "--cpus", "2" ]
		end

		## A private link between host & VM
		## Probably best to use a reserved address, 192.168.0.0/16 or 10.0.0.0/8 
    www.vm.network "private_network", ip: "192.168.13.37"
		
		## Map the local parent folder to /var/www/24ways on the VM
    www.vm.synced_folder "../", "/var/www/24ways",
        	owner: "www-data", group: "www-data"
			
		## This script will be executed on the VM, to install things like Apache & PHP
    www.vm.provision :shell, :path => "provision.sh"

    ## Our symlinked Apache VirtualHost doesn't exist when apache is installed
		## this ensures that the config file is picked up later
    www.vm.provision "shell", inline: "service apache2 restart", run: "always"
    
  end

end
