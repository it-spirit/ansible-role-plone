spirit.plone
============

This role installs and configures a Plone Enterprise CMS system.

Requirements
------------

This role has no requirements.


Role Variables
--------------

TODO.


Dependencies
------------

This role has no dependencies.


Example Playbook
----------------

::

    - hosts: servers
      roles:
         - { role: spirit.plone }


Connecting to the client_reserved client
----------------------------------------

To start the `client_reserved` client, run the following `supervisorctl` command::

    $ ansible my_server -K -b -a "supervisorctl start default_client_reserved"

Next, open a ssh tunnel (remember the `client_reserved` is running on localhost only)::

    $ ssh -L 8000:localhost:8080 my_server_fqdn_or_ip -N

You can also disable host key checking (especially when working with development machines)::

    $ ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -L 8000:localhost:8080 my_server_fqdn_or_ip -N

You can now access your Plone sites in your webbrowser at http://localhost:8000/manage.

To stop the `client_reserved` client again, run the following `supervisorctl` command::

    $ ansible my_server -K -b -a "supervisorctl stop default_client_reserved"


License
-------

BSD


Author Information
------------------

This role was created 2016-2017 by `it-spirit Software <http://it-spir.it>`_.
