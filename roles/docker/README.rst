Docker setup
============

Docker setup Ansible role. This role is largely inspired by `Jeff Geerling <https://github.com/geerlingguy/ansible-role-docker>`_ Docker role, I made my one since I tend to not rely on external projects too much especially when they are quite simple.

Requirements
------------

This role was written for Debian.

Role Variables
--------------

Variables can be found in the `default vars <defaults/main.yml>`_

.. code-block:: yaml

   docker_edition: 'ce'
   docker_dependencies:
     - "apt-transport-https"
     - "ca-certificates"
     - "curl"
     - "gpg"
     - "gnupg"
     - "lsb-release"
   docker_packages:
     - "docker-{{ docker_edition }}"
     - "docker-{{ docker_edition }}-cli"
     - "docker-{{ docker_edition }}-rootless-extras"
     - "containerd.io"

Defines Docker flavor to install, dependencies and the packages to install. We don't install the docker-compose binary since compose is include in the Docker command line.

.. code-block:: yaml

   docker_users:
     - "syrell"

A list of UNIX users to add to the docker group.

.. code-block:: yaml

   docker_daemon_options:
     docker_daemon_options:
       log-opts:
         max-size: "100m"

A dictionary listing options to add to the Docker daemon.

.. code-block:: yaml

   docker_apt_release_channel: stable
   docker_repo_url: https://download.docker.com/linux
   docker_apt_arch: amd64
   docker_apt_gpg_key: "{{ docker_repo_url }}/{{ ansible_distribution | lower }}/gpg"
   docker_apt_repository: "deb [arch={{ docker_apt_arch }}] {{ docker_repo_url }}/{{ ansible_distribution | lower }} {{ ansible_distribution_release }} {{ docker_apt_release_channel }}"

Variables relative to the Docker Debian repository.

Dependencies
------------

None.

Example Playbook
----------------

.. code-block:: yaml

   - name: Install docker
     hosts: all
     roles:
       - docker

License
-------

BSD-3

Author Information
------------------

Role created by `syrell <https://git.syyrell.com/syrell>`_
