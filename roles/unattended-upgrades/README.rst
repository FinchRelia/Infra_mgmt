Unattended-upgrades setup
=========================

Unattended-upgrades setup role. It also installs and configures Postfix as a SMTP relay in order to send emails when target system needs to be rebooted.

Requirements
------------

This role was written for Debian (11) and requires root privileges.

Role Variables
--------------

Variables can be found in the `default vars <defaults/main.yml>`_. As a bare minimum you should configure SMTP credentials.

.. code-block:: yaml

  upgrades_sender: "{{ ansible_user }}@{{ ansible_hostname }}.lan"

Defines which email unattended-upgrades will use to send emails.

.. code-block:: yaml

  postfix_hostname: "{{ ansible_hostname }}.lan"

Configures Postfix hostname.

.. code-block:: yaml

  smtp_username:
  smtp_password:
  smtp_port: 587

SMTP credentials (required). Port defaults to 587 (STARTTLS).

.. code-block::yaml

  relay_servername: "{{ smtp_username | regex_search('(?<=@)(.+)\\.[\\w]+$') }}"

SMTP servername, defaults to ``smtp_username`` domain. If yours differs modify it here.

.. code-block:: yaml

  custom_smtp_header: false
  from_header:
  from_email:

Customizes SMTP header. Make sure to configure ``from_header`` (added header) and ``from_email`` (email address of FROM) correctly if you enable SMTP headers variable.

.. code-block:: yaml

  smtp_masquerade: false

SMTP masquerade allows to replace the FROM statement to the value of ``smtp_username``.

.. code-block:: yaml

  additional_lists: []

List of additional sources lists you want to add to unattended-upgrades.

Dependencies
------------

None.

Example Playbook
----------------

.. code-block:: yaml
  
  - name: Deploy automatic upgrades
    hosts: all
    become: true
    vars:
      smtp_username: user@domain.com
      smtp_password: pa$$word      
    roles:
      - role: 'unattended-upgrades'

License
-------

BSD-3

Author Information
------------------

Role created by `syrell <https://git.syyrell.com/syrell>`_.
