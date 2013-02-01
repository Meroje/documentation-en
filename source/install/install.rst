Installation
############

.. contents::
	:depth: 2

General requirements
********************

* Have a **LAMP** server up and running (with PHP 5.3+).
	.. code-block:: bash

			sudo apt-get install apache2 php5 mysql-server libapache2-mod-php5 php5-mysql

* **mod_rewrite d’Apache** is enabled.
	.. code-block:: bash

			sudo a2enmod rewrite

| These commands are given as an example if you install Novius OS on your local machine or a server for which you have admin rights.
| They are Ubuntu commands, adapt them to your distribution if necessary.


.. note::

	In theory Novius OS can be installed on other servers than **Apache**.

Quick installation
******************

Requirements
============

* Have a command line access to the server and being granted ``sudo`` admin rights.
* **Git** is installed.

Installation
============

Open a terminal and enter:

.. code-block:: bash

    cd /var/www
    sudo wget https://raw.github.com/novius-os/ci/master/0.2/tools/install.sh && sh install.sh

Once the installation complete:

* Open your browser at http://votredomaine/novius-os/ (replace ``novius-os`` with the directory name you've chosen).
* Follow the steps of the :doc:`setup wizard <setup_wizard>`.

.. note::

	* For a local installation, the URL is probably http://localhost/novius-os/ .
	* If your server's ``DOCUMENT_ROOT`` isn't ``/var/www/``, change the first command accordingly.

Installation via Zip file
*************************

We recommend you follow this procedure when installing Novius OS on shared hosting:

* Download  `novius-os.0.2.zip <http://www.novius-os.org/download-novius-os-zip.html>`_.
* Unzip the file.
* Upload (or move) the ``novius-os`` directory to your server's ``DOCUMENT_ROOT`` (using FTP for instance).
* Open your browser at http://votredomaine/novius-os/ (replace ``novius-os`` with the directory name where Novius OS has been unzipped).
* Follow the steps of the :doc:`setup wizard <setup_wizard>`.


Advanced installation
*********************

Coming soon!