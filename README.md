openstack_install
=================

All in one installation script for OpenStack

24th Aug 2012 @jedipunkz

Overview
--------
OpenStack All In One Instrallation Script. These compornents of OpenStack will be
installed on 1 node. nova, glance, keystone, swift.

Precondition
------------

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
    |          | | eth0:0 | 192.168.3.1
    |   Host   | +--------+
    |          |
    |          | +--------+
    |          | |  eth0  | ${HOST_IP}
    +----------+ +--------+
    |
    +----------+
    |   CPE    |
    +----------+

Additional Operation
--------------------

* create OS images
* add image to glance
* create ssh keypair and install that

<http://docs.openstack.org/essex/openstack-compute/starter/os-compute-starterguide-trunk.pdf>
