Vagrant.configure("2") do |config|
	config.vm.box = "ubuntu/trusty64"
	config.vm.provision :shell, path: "bootstrap.sh"
	config.vm.network :forwarded_port, guest: 80, host: 4567
	config.vm.provider "virtualbox" do |v|
	  host = RbConfig::CONFIG['host_os']

	  # Give VM 1/4 system memory & access to all cpu cores on the host
	  if host =~ /darwin/
	    cpus = `sysctl -n hw.ncpu`.to_i
	    # sysctl returns Bytes and we need to convert to MB
	    mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 4
	  elsif host =~ /linux/
	    cpus = `nproc`.to_i
	    # meminfo shows KB and we need to convert to MB
	    mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 4
	  else # sorry Windows folks, I can't help you
	    cpus = 2
	    mem = 1024
	  end

	  v.customize ["modifyvm", :id, "--memory", mem]
	  v.customize ["modifyvm", :id, "--cpus", cpus]
	end
  
end

