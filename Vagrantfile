Vagrant.configure("2") do |config|

  ###############################################

  # define number of servers
  M = 3

  # provision N VMs as servers
  (1..M).each do |i|

    # name the VMs
    config.vm.define "k3sserver#{i}" do |node|

      # which image to use
      node.vm.box = "opensuse/Leap-15.6.x86_64"

      # sizing of the VMs
      node.vm.provider "libvirt" do |lv|
        lv.random_hostname = true
        lv.memory = 1024
        lv.cpus = 2
      end

      # set the hostname
      node.vm.hostname = "k3sserver#{i}"

      # if this is the last machine
      if i == M

        #################################################
        # only target one node to not run ansible twice
        # but do not limit this to this node (set ansible.limit to all)
        node.vm.provision "ansible" do |ansible|
          ansible.compatibility_mode = "2.0"
          ansible.limit = "all"
          ansible.groups = {
            "k3sservers"  => [ "k3sserver1", "k3sserver2", "k3sserver3" ],
          }
          ansible.playbook = "ansible/playbook-vagrant.yml"

        end # node.vm.provision

      node.trigger.after :destroy do |trigger|
        trigger.warn = "Removing ansible/k3s-kubeconfig"
        trigger.run = {inline: "rm -vf ansible/k3s-kubeconfig"}
      end # node.trigger.after

      node.trigger.after :destroy do |trigger|
        trigger.warn = "Removing ansible/k3s-token"
        trigger.run = {inline: "rm -vf ansible/k3s-token"}
      end # node.trigger.after

      end # if-condition last machine

    end # config.vm.define servers

  end # each-loop servers

end # Vagrant.configure
