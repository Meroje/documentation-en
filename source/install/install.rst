Installation
############

.. sidebar:: Table of contents

    .. contents::
        :backlinks: top
        :depth: 2
        :local:

General requirements
********************

Have a serevr with MySQL and PHP 5.3+.

Novius OS can run with:

* :program:`Linux`, :program:`Mac OS` or :program:`Windows` (from Vista)
* :program:`Apache` with **mod_rewrite** enabled or :program:`Nginx`

LAMP
====

We describe following the install process on a server :program:`LAMP` (Linux/Apache/MySQL/PHP), :program:`Debian` type,
for which you have admin rights. Adapt to your configuration.

* Install of **AMP**.

	.. code-block:: bash

			sudo apt-get install apache2 php5 mysql-server libapache2-mod-php5 php5-mysql

* Enable **mod_rewrite** of :program:`Apache`.

	.. code-block:: bash

			sudo a2enmod rewrite

Quick installation
******************

Requirements
============

* Have a command line access to the server and being granted :command:`sudo` admin rights.
* :program:`Git` is installed.

Installation
============

Open a terminal and enter:

.. code-block:: bash

    cd /var/www
    sudo wget http://raw.github.com/novius-os/ci/master/chiba2/tools/install.sh && sh install.sh

Once the installation completes:

* Open your browser at http://your.domain/novius-os/ (replace :file:`novius-os` with the directory name you've chosen).
* Follow the steps of the :doc:`setup wizard <setup_wizard>`.

.. note::

	* For a local installation, the URL is probably http://localhost/novius-os/ .
	* If your server's ``DOCUMENT_ROOT`` isn't :file:`/var/www/`, change the first command accordingly.

Installation via Zip file
*************************

We recommend you follow this procedure when installing Novius OS on shared hosting:

* Download  `novius-os.chiba.2.3.2.zip <http://community.novius-os.org/download-novius-os-zip.html>`__.
* Unzip the file.
* Upload (or move) the :file:`novius-os` directory to your server's ``DOCUMENT_ROOT`` (using FTP for instance).
* Open your browser at http://your.domain/novius-os/ (replace ``novius-os`` with the directory name where Novius OS has been unzipped).
* Follow the steps of the :doc:`setup wizard <setup_wizard>`.


Advanced installation
*********************

Configure a Virtual Host
========================

The following commands are provided as example when installing Novius OS on Ubuntu, you should adapt depending on your distribution.

.. code-block:: bash

	sudo nano /etc/apache2/sites-available/novius-os

| Replace :command:`nano` with any text editor.
| Replace :file:`novius-os` with the name you want for your ``Virtual Host``.

| Copy the following configuration in the file you just opened and save.
| Change the line ``ServerName`` with your domain name when installing on a live server.
| Likewise, change :file:`/var/www/novius-os` with the folder you installed Novius OS into.

.. code-block:: apache

	<VirtualHost *:80>
		DocumentRoot /var/www/novius-os/public
		ServerName   novius-os
		<Directory /var/www/novius-os/public>
			AllowOverride All
			Options FollowSymLinks
		</Directory>
	</VirtualHost>

The default configuration has a :file:`public` folder. This is where the ``DoumentRoot`` should point.

Enable the new ``VirtualHost``:

.. code-block:: bash

	sudo a2ensite novius-os

Then, reload :program:`Apache` to apply the new configuration.

.. code-block:: bash

	sudo service apache2 reload

Configure the :file:`hosts` file, when installing on your computer
------------------------------------------------------------------

If you install Novius OS on your local computer, you must add a line in the :file:`/etc/hosts` file, containing the
value you entered for ``ServerName`` (:file:`novius-os` in the above example).

.. code-block:: bash

	sudo nano /etc/hosts

Add the following line:

.. code-block:: bash

	127.0.0.1   novius-os

Advance installation with Git
=============================

You should clone the repository available on GitHub:

.. code-block:: bash

	git clone --recursive git://github.com/novius-os/novius-os.git

This command downloads the main repository, and its submodules :

* novius-os : Novius OS core, which contains submodules itself (like fuel-core or fuel-orm).
* Several submodules in :file:`local/applications`: blog, news, comments, form, slideshow and other applications.

| The repository default branch is latest stable version of Novius OS.
| New versions will be made available in new branches.

| For now, every dependent repository of ``novius-os/novius-os`` share the exact same version number. It means that
  any application available on our Github exists in the same versions as the core. So if you're using the |version|
  version of ``novius-os/core, then you should also use ``novius-os/app`` in the same |version| version number.

| After the initial ``clone``, if you want to change the Novius OS version you're using, don't forget to update the submodules!
| Here's how to use the latest *nightly* (it's in the ``dev`` branch):

.. code-block:: bash

	cd /var/www/novius-os/
	git checkout dev
	git submodule update --recursive

Installation on Nginx server
============================

Sample of :program:`Nginx` configuration:

.. literalinclude:: nginx.txt
