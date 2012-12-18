First time setup wizard
=======================

:doc:`Novius OS files are installed and the server is configured <install>`. You've done the hardest. Now comes the easy part :-)

Open your favourite web browser to the install.php page and let be guided by the wizard:

* http://novius-os/install.php if you followed the local installation procedure
* http://www.yourdomain.com/install.php for a classic installation on an external server
* http://www.yourhosting.com/novius-os-folder/install.php for an installation in a sub-directory on a shared hosting


Step 1: check pre-requisite
---------------------------

This step should be a formality if you installed Novius OS on a shared hosting.
In other cases, if you see a lot of red, don't worry! The website just needs write permissions in some directory. This step gives you explanations about each directory and which commands to run to fix everything.

.. image:: /how_to/step-1a.png
	:alt: Step 1a

If you don't want to bother, just copy/paste the included command summary at the bottom of the page in a terminal: you're done!

.. image:: /how_to/step-1b.png
	:alt: Step 1b

Step 2: setup the MySQL database
--------------------------------

This step requires an existing database, and an associated user who can write into it. On a shared hosting, those settings are given to you by your hosting company. For other cases, here's how to do, assuming `localhost` is your host:

.. code-block:: sql

    CREATE DATABASE `database_name` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
    GRANT ALL PRIVILEGES ON `database_name`.* TO 'username'@localhost IDENTIFIED BY 'password';
    FLUSH PRIVILEGES;

Just fill in the 4 fields to match your configuration. Please note the database must exists, so you may need to create it before going on.

.. image:: /how_to/step-2.png
	:alt: Step 2

This will create the 2 files *local/config/db.php* and *local/config/crypt.php*

Step 3: create the first administrator account
----------------------------------------------

.. image:: /how_to/step-3.png
	:alt: Step 3


Step 4: finishing up installation
---------------------------------

.. image:: /how_to/step-4.png
	:alt: Step 4



Dive into Novius OS
-------------------

.. image:: /how_to/step-login.png
	:alt: Login Screen

You should see the application manager upon first login (because you're the administrator). This is where you can install the applications you want to use.

.. image:: /how_to/step-appmanager.png
	:alt: Applications manager

* *Blog / News* is a "library" application, required for the applications Blog and News stories to work
* *Comments* is a "librairy" application providing the front-office comments layer for the Blog and News Stories applications
* *Simple Facebook share* et *Simple Twitter share*  are `Data catchers <http://novius-os.github.com/docs/applications.html#sharing>`_ to easilly share your :ref:`Content nuggets <sharing_content-nuggets>` on Facebook and Twitter

You're now swimming in Novius OS now. Have fun!
