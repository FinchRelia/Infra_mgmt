Wireguard setup
===============

Wireguard setup role. This role extends `this codebase <https://github.com/lablabs/ansible-collection-wireguard/tree/main/roles/wireguard>`_ to my needs. It's a bit simpler and adds more idempotence, e.g. when replaying the role to add another client to the server.   

Requirements
------------

This role was written for Debian (11) and requires root privileges. It also requires to have several collections installed on your ansible host you won't necessarily have depending on your Ansible installation:

- ansible.posix
- community.general (iptables_save module)
- ansible.utils (network filters)
- netaddr (python package)

Role Variables
--------------

Variables can be found in the `default vars <defaults/main.yml>`_

.. code-block:: yaml

  wireguard_dir: /etc/wireguard
  wireguard_clients_dir: "{{ wireguard_dir }}/clients"
  wireguard_clients_download_dir: clients/
  wireguard_download_clients: false
  wireguard_serverkeys_download_dir: server/
  wireguard_download_serverkeys: false

Defines basic arborescence to store Wireguard files. ``wireguard_download_clients`` and ``wireguard_download_serverkeys`` can optionally set to true in order to download respectively clients and server's keys from the target host.

.. code-block:: yaml

  wireguard_restore_serverkeys_dir: ""

Use this variable if you want to use pre-existing keys from a directory to bootstrap Wireguard. Must ends with '/'.

.. code-block:: yaml

  wireguard_packages:
  - wireguard

List of packages to install.

.. code-block:: yaml

  wireguard_port: 51810

Port which Wireguard will listen to.

.. code-block:: yaml

  wireguard_hostname: "{{ inventory_hostname }}"

Hostname the client will use to connect to the server.

.. code-block:: yaml

  wireguard_interface: wg0

Interface which will be mounted to the server.

.. code-block:: yaml

  nat_out_interface: eth0

Interface where the traffic will be NATed to on the server.

.. code-block:: yaml

  wireguard_address: 10.213.213.0/24

Subnet definition for the VPN network.

.. code-block:: yaml

  wireguard_keepalive: 25

Uses this if you wanna specify a keepalive value. See `this <https://github.com/pirate/wireguard-docs#persistentkeepalive>`_ for more information on keepalive.

.. code-block:: yaml

  wireguard_peers: []

Lits of peers (clients) you wanna create. You can define specific name, address, allowedIPs, DNS and keepalive for each peer. See playbook below for example.

.. code-block:: yaml

  filter_forward: false
  other_interface:

Set ``filter_forward`` to true and specify an interface name for ``other_interface`` if you wanna drop packets from ``wireguard_interface`` to this interface.

Dependencies
------------

None.

Example Playbook
----------------

.. code-block:: yaml

  - name: Deploy Wireguard
    hosts: wireguard_hosts
    become: true
    vars:
      wireguard_hostname: "mywireguard.server.com"
      wireguard_address: 10.10.10.0/24
      wireguard_peers:
        - name: client_001
          allowed_ip: "0.0.0.0/0, ::/0"
          address: "10.10.10.2"
        - name: client_002
          allowed_ip: "0.0.0.0/0, ::/0"
          address: "10.10.10.3"
    roles:
      - wireguard

License
-------

BSD-3

Author Information
------------------

Role created by `syrell <https://git.syyrell.com/syrell>`_
