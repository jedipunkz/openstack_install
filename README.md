openstack_install
=================

All in one installation script for OpenStack

24th Aug 2012 @jedipunkz

Overview
--------
OpenStack All In One Instrallation Script. These compornents of OpenStack will be
installed on only 1 node. nova, glance, keystone, swift.

Precondition
------------

Node that you want to use for installation must have these environment.

* Ubuntu Server 12.04 LTS amd64
* Intel-VT or AMD-V machine
* 1 NIC or more NICs

Structure
---------

    +--+--+--+
    |VM|VM|VM|  192.168.4.32/27
    +--+--+--+..
    +----------+ +--------+
    |          | | br100  | 192.168.4.33/27 -> floating range : 10.200.8.32/27
    |          | +--------+
    |          | | eth0:0 | 192.168.3.1       disk devices
    |   Host   | +--------+            +------------------------+
    |          |                       | /dev/sda6 nova-volumes |
    |          | +--------+            +------------------------+
    |          | |  eth0  | ${HOST_IP} | /dev/sda7 swift        |
    +----------+ +--------+            +------------------------+
    |              nw I/Fs
    +----------+
    |   CPE    |
    +----------+

Additional Operation
--------------------

#### create OS images

Create a OS Image on this node. for example I show you that operation for
ubuntu server 12.04 LTS image.

    # kvm-image create -f qcow2 server.img 5G
	# wget http://gb.releases.ubuntu.com//precise/ubuntu-12.04-server-amd64.iso
	# kvm -m 256 -cdrom ubuntu-12.04-server-amd64.iso -drive file=server.img,if=virtio,index=0 -boot d -net nic -net user -nographic -vnc :0

Connect VNC to ${node_IP}:0, and install OS on VNC Tool. When you finish
installation operation, re-run kvm command to boot from hard disk.

    # kvm -m 256 -drive file=server.img,if=virtio,index=0 -boot c -net nic -net user -nographic -vnc :0

Re-connect VNC to ${node_IP}:0, and execute this command.

    # sudo rm -rf /etc/udev/rules.d/70-persistent-net.rules
    # shutdown -h now

#### add OS image to glance

Add server.img to glance for OS image template on OpenStack.

    # glance add name="Ubuntu Server 12.04LTS" is_public=true container_format=ovf disk_format=qcow2 < server.img

#### Create ssh keypair and install that

Create SSH-Keypair and install that to OpenStack Nova.

    # ssh-keygen
	# nova keypair-add --pub_key .ssh/id_rsa.pub mykey
	# nova keypair-list

#### Open Dashboard : Horizon

Operation was done. Open http://${node_IP} to use Horizon (OpenStack
Dashboard) and create some VMs.

<http://docs.openstack.org/essex/openstack-compute/starter/os-compute-starterguide-trunk.pdf>
