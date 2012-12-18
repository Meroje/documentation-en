How to install Novius OS
========================

This guide provides instructions to run on Ubuntu. Be sure to adapt the commands for your own OS.



* :ref:`install_download`

	* :ref:`install_download-github`
	* :ref:`install_download-zip`
	
* :ref:`install_server-configuration`

	* :ref:`install_server-dedicated`
	* :ref:`install_server-shared`


.. _install_download:

Step 1: download Novius OS source code
--------------------------------------

.. _install_download-github:

Method A: Git and GitHub
^^^^^^^^^^^^^^^^^^^^^^^^

Step A-1: prerequisites - install GIT
"""""""""""""""""""""""""""""""""""""

::

    sudo apt-get install git

Step A-2: clone the sample website repository
"""""""""""""""""""""""""""""""""""""""""""""

We use submodules, so be sure to fetch them properly. The ``--recursive`` option does everything you need.

::

    cd ~
    git clone --recursive git://github.com/novius-os/novius-os.git
    sudo mv novius-os /var/www/

This will download a sample repository, with several submodules:

* novius-os : the core of Novius OS, which has other submodules, like fuel-core or fuel-orm ;
* Other submodules in the local/applications folder: blog, news and comments.


Step A-3 (optional): change the version you want to use (if you're gutsy)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

We configured the cloning of the repository to point to the latest available release (it's **master/0.1.4*** at the time I'm writing this).

| When we deploy a version, we create a new branch for it.
| For now, we keep synchronised all the dependant repositories. Hence, an application provided on our Github will follow the same version number as the core. So if you're using novius-os/core version 0.3 (not yet released!), you need to use novius-os/app in the same version 0.3 too.

| To change the version you're using after cloning, *don't forget to update the git submodules*.
| Example to use the latest nightly from the 'dev' branch

::

    cd /var/www/novius-os/
    git checkout dev
    git submodule update --recursive

.. _install_download-zip:

Method B: from a .zip file
^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    cd ~
    wget http://nova.li/nos-014 -O novius-os.0.1.4.zip
    unzip novius-os.0.1.4.zip
    sudo mv novius-os /var/www/

Or download the file `nos-014 <http://nova.li/nos-014>`_ and unzip it with your favourite program.

.. _install_server-configuration:

Step 2: Server configuration
----------------------------

We'll be discussing 2 cases:

* installation on a server you control (either your local machine, a VM or a dedicated external server) ;
* installation on a shared hosting service, without SSH command-line access or the possibility to change Apache configuration.

.. _install_server-dedicated:

Method A: you control the server
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Step A-1: make sure Apache's mod_rewrite is enabled
"""""""""""""""""""""""""""""""""""""""""""""""""""

::

    sudo a2enmod rewrite

Step A-2: configure a VirtualHost
"""""""""""""""""""""""""""""""""

Create a new ``VirtualHost`` for Novius OS (replace ``nano`` with your favourite text editor in the following commands)

::

    sudo nano /etc/apache2/sites-available/novius-os

Copy the following configuration in the file, and save it. Adapt the ``ServerName`` line with your own domain when installing on a production server.

::

    <VirtualHost *:80>
        DocumentRoot /var/www/novius-os/public
        ServerName   novius-os
        <Directory /var/www/novius-os/public>
            AllowOverride All
            Options FollowSymLinks
        </Directory>
    </VirtualHost>

The default configuration contains a *public* directory. The webroot should points to this directory.


Enable the freshly created ``VirtualHost``

::

    sudo a2ensite novius-os


Reload Apache (or your other web server) to take the new configuration into account.

::

    sudo service apache2 reload


Step A-3: configure the ``hosts`` file, when installing on your local machine
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

If the ``ServerName`` is different than ``localhost`` (``novius-os`` in the above example), you should add the server name into your ``hosts`` file.

::

    sudo nano /etc/hosts


Add the following line:

::

    127.0.0.1   novius-os


.. _install_server-shared:

Method B: Shared hosting
^^^^^^^^^^^^^^^^^^^^^^^^

Step B-1: upload the source code to your server
"""""""""""""""""""""""""""""""""""""""""""""""

You can choose the way you do it, depending on your shared hosting provider (FTP, SSH, Git...)

Step B-2: ``.htaccess`` files
"""""""""""""""""""""""""""""

Novius OS needs an ``.htaccess`` file to run.

In a classic installation, the ``DOCUMENT_ROOT`` should point to the ``public`` directory of Novius OS (see step A-2 above). On a shared hosting, you don't choose the location for ``DOCUMENT_ROOT``. So you need to delete the ``public/.htaccess`` file and rename the ``.htaccess.shared-hosting`` inside the Novius OS root folder into ``.htaccess``.

Then, edit this ``.htaccess`` file, and change the line beginning with ``ErrorDocument`` depending on where you installed Novius OS::

    ErrorDocument 404 /novius-os-install-dir/public/htdocs/novius-os/404.php

If Novius OS has been installed in the root directory of your hosting::

    ErrorDocument 404 /public/htdocs/novius-os/404.php


Step B-3: ``local/config/config.php`` file
""""""""""""""""""""""""""""""""""""""""""

Edit the ``local/config/config.php`` file, un-comment and adapt the following line to your case::

    'base_url' => 'http://www.yourdomain.com/novius-os-install-dir/',


Step 3: Finish the installation
-------------------------------

You've done the hardest. Now you just need to go through the :doc:`setup-wizard` to enjoy your Novius OS.
