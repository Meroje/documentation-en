Common Installation Problems
############################

.. sidebar:: Table of contents

    .. contents::
        :backlinks: top
        :depth: 2
        :local:

Htaccess not allowed on Apache server
*************************************

Symptoms
--------

* You have a message ``Your server does not allow .htaccess file``
* You run Novius Os on an Apache server
* You've installed Novius OS in a subfolder of a host, classically the default host

Workaround
----------

* Find the virtualhost config file, commonly in :file:`/etc/apache2/site-enabled/`.
* Edit the virtualhost config file with write permission.

In this example, we use the :program:`nano` editor and the virtualhost config file name is :file:`000-default` :

.. code-block:: bash

    sudo nano /etc/apache2/site-enabled/000-default

In the file, find a line like this (if Novius OS is installed in a :file:`/var/www/` subdirectory):

.. code-block:: apache

    <Directory /var/www>
        AllowOverride None
        Options FollowSymLinks
    </Directory>

Change ``AllowOverride None`` by ``AllowOverride All``. Save your change and restart Apache:

.. code-block:: bash

    sudo service apache2 restart

Write permissions on Windows system
***********************************

Symptoms
--------

* You've installed Novius OS on Windows
* You have messages beginning like ``Give write permission to all users``

Workaround
----------

You can try to run your WAMP server with administrator privileges.

Or you can try to change file permission on Novius OS directory, recursively on all subfolders.
Give write access for everybody (`Example for windows 7 <http://www.wikihow.com/Change-File-Permissions-on-Windows-7>`__).
Maybe restart server after.


Write permissions with FTP
**************************

Symptoms
--------

* You've installed Novius Os by uploading it by FTP
* You have messages saying that some directories ``must be writeable``
* You can not execute commands given, you can't access server by :program:`ssh`

Workaround
----------

You can give write permissions with your FTP client. For example, a `tuto for Filezilla <http://www.dummies.com/how-to/content/how-to-change-file-permissions-using-filezilla-on-.html>`__

``chmod a+w`` means give write permissions for all users.

GD installation on Ubuntu
*************************

Symptoms
--------

* You've message saying that ``GD is required``
* You run Novius OS on Ubuntu

Workaround
----------

.. code-block:: bash

    sudo apt-get install php5-gd
    sudo apt-get install libgd2-xpm-dev*

Json extension not installed
****************************

Symptoms
--------

* You've message saying that ``Call to undefined function json_encode()`` or ``Call to undefined function json_decode()``

Some distributions have removed the standard JSON extension as of PHP 5.5rc2 due to a license conflict.

Workaround
----------

.. code-block:: bash

     sudo apt-get install php5-json

Forbidden when access back-office
*********************************

Symptoms
--------

* After install wizard, when you try to access to back-office, your browser send you a page saying ``Forbidden``

This problem exists for Web hoster ``Infomaniak.ch``

Workaround
----------

Edit :file:`.htaccess` file. Change this line:


.. code-block:: apache

    Options +FollowSymLinks -Indexes

By:

.. code-block:: apache

    Options +FollowSymlinks -SymlinksIfOwnerMatch -Indexes


magic_quotes_gpc mus be off
***************************

Symptoms
--------

* You have the message saying ``PHP configuration directive ‘magic_quotes_gpc’ must be off``
* You've use ``OVH`` Web hosting

Workaround
----------

Add this line in the :file:`.htaccess` file:

.. code-block:: apache

    SetEnv MAGIC_QUOTES 0
