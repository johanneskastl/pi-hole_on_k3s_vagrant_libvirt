# pi-hole_on_k3s_vagrant_libvirt

Vagrant-libvirt setup that creates three VMs with [k3s](https://k3s.io/), the
minimal lightweight Kubernetes distribution. All three machines are running as
controlplane nodes, but are also running user workloads.

On top of k3s, a single [Pi-Hole](https://pi-hole.net/) instance is being
deployed.

Default OS is openSUSE Leap 15.6, but that can be changed in the Vagrantfile.
Same holds true for the sizing of the machines.

## Vagrant

1. You need vagrant obviously.
1. Fetch the box, per default this is `opensuse/Leap-15.6.x86_64`, using
   `vagrant box add opensuse/Leap-15.6.x86_64`.
1. Make sure the git submodules are fully working by issuing `git submodule init
   && git submodule update`
1. Run `vagrant up`
1. Open the URL that Ansible prints out at the end of the provisioning, and log
   in using the username `admin` with the password `totallynotsecure`.
   (The URL should look something like `http://pihole.192.0.2.13.sslip.io`).
1. You can also test a DNS query using e.g. `dig` or `drill` or `doggo` etc.

   ```
   dig @192.0.2.13 opensuse.org
   ```

1. Party!

## Cleaning up

The VMs can be torn down after playing around using `vagrant destroy`. This will
also remove the kubeconfig file `ansible/k3s-kubeconfig` and the token file
`ansible/k3s-token`.

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via git@johannes-kastl.de
