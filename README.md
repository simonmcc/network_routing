network_routing
=================

_WARNING: This role can be dangerous to use. If you lose network connectivity
to your target host by incorrectly configuring your networking, you may be
unable to recover without physical access to the machine._

This roles enables users to configure various routing details on target
machines. The role can be used to configure:

- source based routing via iproute2 (see [lartc](http://lartc.org/howto/lartc.rpdb.html) for more info on policy based routing)
- other techniques & policies to be added as required

Requirements
------------

This role requires Ansible 1.4 or higher, and platform requirements are listed
in the metadata file.

Role Variables
--------------

The variables that can be passed to this role and a brief description about
them are as follows:

    # The list of source based rules to be added to the system
    network_source_routing: []

Note: The values for the list are listed in the examples below.

Examples
--------

1) Configure eth1 and eth2 on a host with a static IP and a dhcp IP. Also
define static routes and a gateway.

	- hosts: myhost
      roles:
        - role: network_routing
          network_source_routing:
  		    - rt_table_id: 200
		      rt_table_name: public_source
		      source_dev: eth2
		      # traffic comes in on eth2, eth2 has 15.126.57.4
		      # so traffic should go back out the same interface
		      source_dev_ip: 15.126.57.4
		      source_gw: 15.126.57.1


2) All the above examples show how to configure a single host, The below
example shows how to define your network configurations for all your machines.

Assume your host inventory is as follows:

### /etc/ansible/hosts

    [dc1]
    host1
    host2

Describe your network configuration for each host in host vars:

### host_vars/host1
	
	network_source_routing:
  	  - rt_table_id: 200
	    rt_table_name: public_source
		source_dev: eth2
		# traffic comes in on eth2, eth2 has 15.126.57.4
		# so traffic should go back out the same interface
		source_dev_ip: 15.126.58.4
		source_gw: 15.126.58.1

### host_vars/host2

	network_source_routing:
  	  - rt_table_id: 200
		rt_table_name: public_source
		source_dev: eth2
		# traffic comes in on eth2, eth2 has 15.126.57.4
		# so traffic should go back out the same interface
		source_dev_ip: 15.126.57.4
		source_gw: 15.126.57.1

Create a playbook which applies this role to all hosts as shown below, and run
the playbook. All the servers should have their network routing configured
as desired.

    - hosts: all
      roles:
        - role: network

Note: Ansible needs network connectivity throughout the playbook process, you
may need to have a control interface that you do *not* modify using this
method so that Ansible has a stable connection to configure the target
systems.

Dependencies
------------

None

License
-------

Apache

Author Information
------------------

Simon McCartney
(inspired by [https://github.com/khappone/network_interface](https://github.com/khappone/network_interface) & [https://github.com/bennojoy/network_interface](https://github.com/bennojoy/network_interface)

