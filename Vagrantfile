# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
	# This is the vagrant-vsphere configuration. 
	# Before using this, make sure to run "vagrant plugin install vagrant-vsphere"

  config.vm.box = "dummy"

  config.vm.box_url = "./dummy.box"
  # This box was built by creating a metadata.json file with these contents: 
  # {
  #   "provider": "vsphere"
  # }
  # EOF
  # and then running tar czvf dummy.box metadata.json 

  config.vm.provider :vsphere do |vsphere|
	  # The vCenter Server we'll be connecting to
	  vsphere.host = 'OURHOST'

	  # The resource pool for the VM 
	  # Note - it isn't a bad idea to give individual users their own resource groups
	  # to spin things up in, so that you can control resource utilization
	  vsphere.resource_pool_name = 'MyResourcePool'

	  # In our environment, we're cloning from VMs directly. This is because I am lazy
	  # and don't want to do the whole clone/update/re-create-template dance when 
	  # I want to update a source image
	  vsphere.clone_from_vm = true
	  
	  # It's important to give the full path in the folder structure, otherwise 
	  # you get a cryptic error when trying to spin up
	  vsphere.template_name = 'path/to/template'

	  # This is the name of the new VM in vSphere
	  vsphere.name = 'VM Instance Name' 

	  # vCenter username to log into the server. Remember to specify domain if necessary
	  vsphere.user = 'DOMAIN\username'

	  # vCenter Password. Comment out to be prompted each time. 
	  # vsphere.password = 'IfYouAreIntoThatSortOfThing'

	  # Ain't nobody got time to get all of the cert stuff setup correctly. 
	  # OK, I really should, but #SoMuchWork
	  vsphere.insecure = true

	  # Pretty obvious. Note: people can do dumb things. Make sure you're ok
	  # on diskspace before you give this to users.
	  vsphere.data_store_name = 'MyDatastoreName'
	  
	  # The difference between a linked clone and a full clone is that a linked-clone 
	  # doesn't need to be fully copied each time. The blocks are copy-on-write, which
	  # means that it spins up much faster 
	  vsphere.linked_clone = true 

  end

  # Make sure that the shell provioner script is updated to reflect your 
  # OS of choice (yum vs apt-get and so on)
	config.vm.provision :shell, :path => "bootstrap.sh"
	config.vm.provision :puppet do |puppet|
		puppet.manifests_path = "puppet/manifests"
		puppet.module_path = "puppet/modules"
		puppet.manifest_file = "init.pp"
	end
  end

