# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :otuslinux1 => {
        :ip_addr => '192.168.11.111',
        :box_name => "mikev1963/centos7",
	# VM CPU count
        :cpus => 2,
        # VM RAM size (Mb)
        :memory => 1024,
        # forwarded ports
        :forwarded_port => []
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
    # Apply VM config
      config.vm.define boxname do |box|
          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s
          # Port-forward config if present
          if boxconfig.key?(:forwarded_port)
            boxconfig[:forwarded_port].each do |port|
              box.vm.network "forwarded_port", port
            end
          end
          box.vm.network "private_network", ip: boxconfig[:ip_addr]

          box.vm.provider "virtualbox" do |v|
            # Set VM RAM size and CPU count
            v.memory = boxconfig[:memory]
            v.cpus = boxconfig[:cpus]
          end
          box.vm.provision "shell", inline: <<-SHELL
            mkdir -p ~root/.ssh
            cp ~vagrant/.ssh/auth* ~root/.ssh
            yum install -y mdadm smartmontools hdparm gdisk
            systemctl restart sshd
          SHELL
      end
  end
end

